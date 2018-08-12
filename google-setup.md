Self Driving Car Nanodegree

Slack : https://carnd.slack.com/messages/C3V1W5H25/convo/C879DQ5JT-1520443102.000618/
Resource Guide : https://sites.google.com/knowlabs.com/self-driving-car/
Deadlines : https://docs.google.com/spreadsheets/d/12ipd3BmKD5aaCaKOtuBsQ6Bh6yM7TQ99uO5SxxawoQo/edit#gid=0
Weekly Outline : https://s3-us-west-2.amazonaws.com/udacity-email/Documents/SDC+Weekly+Syllabus+-+March+2018+Class.pdf#SDCSlack
Slack help : https://sites.google.com/knowlabs.com/intro-self-driving-cars/slack?authuser=0
Comma.ai : http://research.comma.ai/
	
	
Starter Kit : https://github.com/udacity/CarND-Term1-Starter-Kit
https://github.com/jbhuang0604/awesome-computer-vision


1. Spin up an instance with GPU and tensorflow


Spin up a Tensorflow VM : https://console.cloud.google.com/compute/instances?filter=solution-type:vm&q=tensor&subtask=details&subtaskValue=click-to-deploy-images%2Fdeeplearning&project=warm-ring-191516&landing=true

CLI : https://cloud.google.com/deep-learning-vm/docs/cli

	export IMAGE_FAMILY="tf-latest-cu92"
	export ZONE="us-west1-b"
	export INSTANCE_NAME="my-instance"
	
	gcloud compute instances create $INSTANCE_NAME \
	  --zone=$ZONE \
	  --image-family=$IMAGE_FAMILY \
	  --image-project=deeplearning-platform-release \
	  --maintenance-policy=TERMINATE \
	  --accelerator='type=nvidia-tesla-v100,count=1' \
	  --metadata='install-nvidia-driver=True'
	

2. Open firewall port (and tag the machine above with http-server network tag)

		gcloud compute --project=warm-ring-191516 firewall-rules create default-allow-jupyter --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:8080 --source-ranges=0.0.0.0/0 --target-tags=http-server
		

3. SSH and install dependencies
	
		gcloud compute ssh --project warm-ring-191516 --zone us-central1-a tensorflow-1-vm

		git clone https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project
		 
		git clone https://github.com/udacity/CarND-LeNet-Lab
		git clone https://github.com/udacity/CarND-Term1-Starter-Kit
		
		sudo apt-get install bzip2
		
		wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
		
		chmod +x Miniconda3-latest-Linux-x86_64.sh
		./Miniconda3-latest-Linux-x86_64.sh
		
		sudo apt-get install tmux
		tmux 
		cd CarND-Term1-Starter-Kit
		
		conda env create -f environment-gpu.yml
		conda activate carnd-term1
		conda install jupyterlab
		jupyter lab


4. Edit jupyter notebook and add password.


	Edit `sudo vim /root/.jupyter/jupyter_notebook_config.py` and add password

			aaMNLmySQgBMvqUUDfDD4nGPHByUQ


5. To stop and start the instance


		gcloud compute instances start --project warm-ring-191516 --zone us-central1-a tensorflow-1-vm
		gcloud compute instances stop --project warm-ring-191516 --zone us-central1-a tensorflow-1-vm

6. To get the public IP of the instance
	
		$ gcloud --format="value(networkInterfaces[0].accessConfigs[0].natIP)" compute instances list
		gcloud --format="value(networkInterfaces[0].accessConfigs[0].natIP)" compute instances describe tensorflow-1-vm

		open http://$(gcloud --format='value(networkInterfaces[0].accessConfigs[0].natIP)' compute instances describe tensorflow-1-vm):8080/?token=c57cfa85f562081af3c3caea0f7db6a7553e39c31200f302


- Git SSH key setup

		eval "$(ssh-agent -s)"
		ssh-add ~/gitkey
