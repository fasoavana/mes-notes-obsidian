
dockerfile

FROM ubuntu:22.04
RUN apt-get install && apt-get update -y python3
RUN apt-get install && apt-get update -y python3
WORKDIR /app
COPY . .
CMD["python","app.py"]

docker-compose

services:
	app.py:
	 build : .
	 port:
		  - "${PORT} :80"


JENKINSFILE

pipeline{
	agent any
	 stages{
		 stage(build){
			 steps{
				  sh"docker-compose up -d --build"
			 }
			
		 }
	 }
	 triggers{
	 pollSCM(* * * * * )
	 }

}