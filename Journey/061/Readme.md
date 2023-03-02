# Dynamically watermark images using Amazon S3 Object Lambda as they are retrieved

## Introduction

Use S3 Object Lambda to add a watermark to an image as it is retrieved from Amazon S3. S3 Object Lambda can be used to modify data as it is retrieved from Amazon S3, without changing the existing object or maintaining multiple derivative copies of the data. By presenting multiple views of the same data and removing the need to store derivative copies, you can save on storage costs.

## Prerequisite

AWS account.

## Use Case

In certain cases, images may need to be modified to include a watermark depending on the user accessing the image (for example, a paying subscriber can view images without watermarks, while a non-paying user receives a watermarked image).

## Services Covered

- S3
- CloudShell (Lambda, IAM)


## Try yourself

### Step 1 — S3 Bucket
- Go to the S3 console and create a bucket.
    - Give a name and select the region.
    - Leave the rest all as default configurations.
    - Create bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.0.JPG)

- Upload an object.
    - Go to the Object tab in the bucket.
    - Choose Upload.
    - Choose Add files and select any image you want to watermark.
    - Upload.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.1.5.JPG)

### Step 2 — S3 Access Point
- Go to the Access Points menu option in the left side menu.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.2.JPG)

- Choose the Create access point button.
- In the Properties section, 
    - Enter a desired Access point name.
    - Choose the Bucket created by selecting the Browse S3 button. 
    - Set the Network origin as Internet.
    - Keep all other defaults as it is. 
    - Create access point button.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.3.JPG)

### Step 3 — Lambda Function and IAM Role using CloudShell
- Go to the CloudShell console.
- Prepare CloudShell to deploy the Lambda function
    - Run the following code in the CloudShell to prepare the environment and deploy the Lambda layer with the Pillow module.
        ```
            # Install the required libraries to build new python
            sudo yum install gcc openssl-devel bzip2-devel libffi-devel -y
            # Install Pyenv
            curl https://pyenv.run | bash
            echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
            echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
            echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
            source ~/.bash_profile

            # Install Python version 3.9
            pyenv install 3.9.13
            pyenv global 3.9.13

            # Build the pillow Lambda layer
            mkdir python
            cd python
            pip install pillow -t .
            cd ..
            zip -r9 pillow.zip python/
            aws lambda publish-layer-version \
                --layer-name Pillow \
                --description "Python Image Library" \
                --license-info "HPND" \
                --zip-file fileb://pillow.zip \
                --compatible-runtimes python3.9
        ```
- Build the Lambda function.
    - Download a TrueType font that will be used by the Lambda function to add a watermark to an image. Copy and paste the following commands into CloudShell.
        ```
            wget https://m.media-amazon.com/images/G/01/mobile-apps/dex/alexa/branding/Amazon_Typefaces_Complete_Font_Set_Mar2020.zip
        ```
    - Extract the TrueType font that will be used to write the watermarked text in the image.
        ```
            unzip -oj Amazon_Typefaces_Complete_Font_Set_Mar2020.zip "Amazon_Typefaces_Complete_Font_Set_Mar2020/Ember/AmazonEmber_Rg.ttf"
        ```
    - Create the Lambda code that will be used to process the S3 Object Lambda requests. Change the text in the quotes ('Watermark') in img_result = add_watermark(img, parsed_qs.get('X-Amz-watermark', ['Watermark'])[0]) to any text you want as the watermark to be.
        ```
            cat << EOF > lambda.py
            import boto3
            import json
            import os
            import logging
            from io import BytesIO
            from PIL import Image, ImageDraw, ImageFont
            from urllib import request
            from urllib.parse import urlparse, parse_qs, unquote
            from urllib.error import HTTPError
            from typing import Optional

            logger = logging.getLogger('S3-img-processing')
            logger.addHandler(logging.StreamHandler())
            logger.setLevel(getattr(logging, os.getenv('LOG_LEVEL', 'INFO')))
            FILE_EXT = {
                'JPEG': ['.jpg', '.jpeg'],
                'PNG': ['.png'],
                'TIFF': ['.tif']
            }
            OPACITY = 64  # 0 = transparent and 255 = full solid


            def get_img_encoding(file_ext: str) -> Optional[str]:
                result = None
                for key, value in FILE_EXT.items():
                    if file_ext in value:
                        result = key
                        break
                return result


            def add_watermark(img: Image, text: str) -> Image:
                font = ImageFont.truetype("AmazonEmber_Rg.ttf", 82)
                txt = Image.new('RGBA', img.size, (255, 255, 255, 0))
                if img.mode != 'RGBA':
                    image = img.convert('RGBA')
                else:
                    image = img

                d = ImageDraw.Draw(txt)
                # Positioning Text
                width, height = image.size
                text_width, text_height = d.textsize(text, font)
                x = width / 2 - text_width / 2
                y = height / 2 - text_height / 2
                # Applying Text
                d.text((x, y), text, fill=(255, 255, 255, OPACITY), font=font)
                # Combining Original Image with Text and Saving
                watermarked = Image.alpha_composite(image, txt)
                return watermarked


            def handler(event, context) -> dict:
                logger.debug(json.dumps(event))
                object_context = event["getObjectContext"]
                # Get the presigned URL to fetch the requested original object
                # from S3
                s3_url = object_context["inputS3Url"]
                # Extract the route and request token from the input context
                request_route = object_context["outputRoute"]
                request_token = object_context["outputToken"]
                parsed_url = urlparse(event['userRequest']['url'])
                object_key = parsed_url.path
                logger.info(f'Object to retrieve: {object_key}')
                parsed_qs = parse_qs(parsed_url.query)
                for k, v in parsed_qs.items():
                    parsed_qs[k][0] = unquote(v[0])

                filename = os.path.splitext(os.path.basename(object_key))
                # Get the original S3 object using the presigned URL
                req = request.Request(s3_url)
                try:
                    response = request.urlopen(req)
                except HTTPError as e:
                    logger.info(f'Error downloading the object. Error code: {e.code}')
                    logger.exception(e.read())
                    return {'status_code': e.code}

                if encoding := get_img_encoding(filename[1].lower()):
                    logger.info(f'Compatible Image format found! Processing image: {"".join(filename)}')
                    img = Image.open(response)
                    logger.debug(f'Image format: {img.format}')
                    logger.debug(f'Image mode: {img.mode}')
                    logger.debug(f'Image Width: {img.width}')
                    logger.debug(f'Image Height: {img.height}')

                    img_result = add_watermark(img, parsed_qs.get('X-Amz-watermark', ['Watermark'])[0])
                    img_bytes = BytesIO()

                    if img.mode != 'RGBA':
                        # Watermark added an Alpha channel that is not compatible with JPEG. We need to convert to RGB to save
                        img_result = img_result.convert('RGB')
                        img_result.save(img_bytes, format='JPEG')
                    else:
                        # Will use the original image format (PNG, GIF, TIFF, etc.)
                        img_result.save(img_bytes, encoding)
                    img_bytes.seek(0)
                    transformed_object = img_bytes.read()

                else:
                    logger.info(f'File format not compatible. Bypass file: {"".join(filename)}')
                    transformed_object = response.read()

                # Write object back to S3 Object Lambda
                s3 = boto3.client('s3')
                # The WriteGetObjectResponse API sends the transformed data
                if os.getenv('AWS_EXECUTION_ENV'):
                    s3.write_get_object_response(
                        Body=transformed_object,
                        RequestRoute=request_route,
                        RequestToken=request_token)
                else:
                    # Running in a local environment. Saving the file locally
                    with open(f'myImage{filename[1]}', 'wb') as f:
                        logger.debug(f'Writing file: myImage{filename[1]} to the local filesystem')
                        f.write(transformed_object)

                # Exit the Lambda function: return the status code
                return {'status_code': 200}
            EOF
        ```
    - Create the Lambda zip file that contains the Python code and TrueType font file.
        ```
            zip -r9 lambda.zip lambda.py AmazonEmber_Rg.ttf
        ```
    - Create the IAM role that attaches to the Lambda function.
        ```
            aws iam create-role --role-name ol-lambda-images --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{"Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'
        ```
    - Attach a predefined IAM policy to the IAM role created previously. This policy contains the minimum permissions required to run the Lambda function.
        ```
            aws iam attach-role-policy --role-name ol-lambda-images --policy-arn arn:aws:iam::aws:policy/service-role/AmazonS3ObjectLambdaExecutionRolePolicy

            export OL_LAMBDA_ROLE=$(aws iam get-role --role-name ol-lambda-images | jq -r .Role.Arn)

            export LAMBDA_LAYER=$(aws lambda list-layers --query 'Layers[?contains(LayerName, `Pillow`) == `true`].LatestMatchingVersion.LayerVersionArn' | jq -r .[])
        ```
    - Create and upload the Lambda function.
        ```
            aws lambda create-function --function-name ol_image_processing \
                --zip-file fileb://lambda.zip --handler lambda.handler --runtime python3.9 \
                --role $OL_LAMBDA_ROLE \
                --layers $LAMBDA_LAYER \
                --memory-size 1024
        ```

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.4.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.4.5.JPG)

### Step 4 — S3 Object Lambda Access Point
- Go to the Object Lambda Access Points in the left side menu in S3 console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.5.JPG)

- Choose the Create Object Lambda Access Point button.
- In the General section, for the Object Lambda Access Point name, enter ol-amazon-s3-images-guide.
- Ensure that the AWS Region for the S3 Object Lambda Access Point matches the AWS Region you specified when creating the S3 bucket.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.6.JPG)

- For the Supporting Access Point, specify the Amazon Resource Name (ARN) of the S3 Access Point using the Browse S3 button.
- Under Transformation configuration, in the S3 APIs list, select the GetObject option.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.7.JPG)

- Under Lambda function, select the Lambda function created (ol_image_processing).
- Create Object Lambda Access Point.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.8.JPG)

### Step 5 — Testing and Cleanup
- Select the S3 Object Lambda Access Point that was created.
- Select the image that and then choose the Open button.
- A new browser tab will open with your image and a watermark.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/061/day61.9.JPG)

Delete the S3 Object Lambda Access Point, S3 Access Point, Lambda Function, Empty and Delete S3 bucket to cleanup

## ☁️ Cloud Outcome

Created a Lambda function and used Amazon S3 Object Lambda to dynamically watermark images as they are retrieved.

## Social Proof

[Blog](https://dev.to/aaditunni/dynamically-watermark-images-using-amazon-s3-object-lambda-as-they-are-retrieved-3cm2)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7037166286723833856-1oX5?utm_source=share&utm_medium=member_desktop)
