## Docker build 
cd newapproach
export CONTROLLER_URL=https://controller.showcase-env-dev.project-sparrow.au1.staxapp.cloud:8443/1.4/install/controller/

export API_KEY='c9a9e2687506f924c5ea8707db913722' 

export ECR_REPO_URL=479774528394.dkr.ecr.ap-southeast-2.amazonaws.com/nginx-agent-1


docker build -t nginx-plus ../nginx-plus/.


docker build --build-arg CONTROLLER_URL=$CONTROLLER_URL --build-arg API_KEY=$API_KEY  -t nginx-agent-1 .


export AWS_DEFAULT_PROFILE=sparrow-dev
$(aws ecr get-login --no-include-email --region ap-southeast-2)

docker tag nginx-agent-1:latest ${ECR_REPO_URL}:latest

docker push ${ECR_REPO_URL}:latest

## Docker Run

docker run --hostname local-apigw -p 80:80 -e API_KEY=$API_KEY -d  nginx-agent-1


## Push to AWS

$(aws ecr get-login --no-include-email --region ap-southeast-2)

docker tag nginx-agent-1:latest $ECR_REPO_URL:latest

docker push $ECR_REPO_URL:latest


