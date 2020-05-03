# automation-test-kubernetes

Create your own cluster inside project (using default set up) i.e cluster-1	us-central1-c	3	3 vCPUs	11.25 GB	

Go to your cloud sheel:

git clone https://github.com/TechPrimers/spring-boot-lazy-init-example.git

cd spring-boot-lazy-init-example

Create the docker image:

./mvnw com.google.cloud.tools:jib-maven-plugin:build -Dimage=gcr.io/$GOOGLE_CLOUD_PROJECT/spring-boot-example:v1

Make sure tag is unique, so next time if there is changes, previous tag can be used again. i.e . v1, v2 etc

If you check container registry, then image will be generated. i.e. spring-boot-example.

Now we need to push the image from container resgistry to one of pot.

gcloud container clusters get-credentials cluster-1 --zone us-central1-c

Output:
Fetching cluster endpoint and auth data.
kubeconfig entry generated for cluster-1.

To show list of pods that are running, but it will not give any result since we are not assign any image to pod yet.
kubectl get pods 

To run the app using docker from google cloud: 
docker run -ti --rm -p 8080:8080 gcr.io/$GOOGLE_CLOUD_PROJECT/spring-boot-example:v1

Deploy using kubernetes:

kubectl run spring-boot-example --image=gcr.io/$GOOGLE_CLOUD_PROJECT/spring-boot-example:v1 --port=8080
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/spring-boot-example created

Expose to load balancer:

kubectl expose deployment spring-boot-example --type=LoadBalancer

checked the deployment:
kubectl get deployments

Checked the pods:
kubectl get pods 

Checed the expernal ip of the service:
kubectl get services

Output:
spring-boot-example   LoadBalancer   10.8.1.229   35.193.253.134   8080:31948/TCP   62s
