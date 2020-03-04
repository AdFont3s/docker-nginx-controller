docker build -t nginx-plus nginx-plus/.

docker build --build-arg CONTROLLER_URL=https://controller.showcase-env-dev.project-sparrow.au1.staxapp.cloud:8443/1.4/install/controller/ --build-arg API_KEY='c9a9e2687506f924c5ea8707db913722' -t nginx-agent-1 .


export AWS_DEFAULT_PROFILE=sparrow-dev
$(aws ecr get-login --no-include-email --region ap-southeast-2)
docker tag nginx-agent-1:latest 479774528394.dkr.ecr.ap-southeast-2.amazonaws.com/nginx-agent-1:latest

docker push 479774528394.dkr.ecr.ap-southeast-2.amazonaws.com/nginx-agent-1:latest


docker run --hostname local-apigw -p 80:80 -e API_KEY='c9a9e2687506f924c5ea8707db913722' -d  nginx-agent-1