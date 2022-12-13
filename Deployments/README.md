1. Create a deployment called webapp with image nginx with 5 replicas
    a. Use the below command to create a yaml file.
     i. kubectl create deploy webapp --image=nginx --dry-run -o yaml > webapp.yaml
     ii. Edit it and add 5 replicas

    answer:
 
        webapp5.yaml

2. Get the deployment rollout status 

    answer: 

   k    ubectl rollout status deployment webapp  

       -- deployment "webapp" successfully rolled out

3. Get the replicaset that created with this deployment

    answer:

        kubectl get rs 

4. EXPORT the yaml of the replicaset and pods of this deployment

    answer:

        kubectl get rs webapp-6684ccd7b8 -o yaml > rs-webapp.yaml 

        kubectl get po -o yaml > webapp-pods.yaml

5. Delete the deployment you just created and watch all the pods are also being
    deleted

    answer:

        kubectl delete deployment webapp 

        kubectl get po -w

6. Create a deployment of webapp with image nginx:1.17.1 with container port 80 and
     verify the image version

    a. kubectl create deploy webapp --image=nginx:1.17.1 --dry-run -o yaml >
    webapp.yaml 
    done

    b. add the port section -80- and create the deployment - vi webapp.yaml done

    answer: 

        webapp.yaml 

        kubectl apply -f webapp.yaml

7. Update the deployment with the image version 1.17.4 and verify

    answer:

        kubectl set image deployment webapp nginx=nginx:1.17.4 --record=true 

        kubectl describe deployment webapp | grep -i annotations

        kubectl rollout status deployment/webapp

8. Check the rollout history and make sure everything is ok after the update

    answer:

        kubectl rollout history deploy/webapp

9. Undo the deployment to the previous version 1.17.1 and verify Image has the
    previous version

    answer:

        kubectl rollout undo deploy/webapp

        kubectl rollout status deployment/webapp

        kubectl describe deployment webapp | grep -i image

10. Update the deployment with the wrong image version 1.100 and verify something is
    wrong with the deployment

        kubectl set image deployment webapp nginx=nginx:1.100 --record=true

    a. Expect: kubectl get pods -ImagePullErr-

        kubectl get po -- webapp-77b685f94f-dhhcl   0/1     ErrImagePull

    b. Undo the deployment with the previous version and verify everything is Ok

        kubectl rollout undo deploy/webapp
        kubectl get deploy
        kubectl rollout history deploy/webapp
        kubectl get po webapp-684ff84dd7-rzp46   1/1     Running

    c. kubectl rollout history deploy webapp --revision=7

        error: unable to find the specified revision

    d. Check the history of the specific revision of that deployment

    answer:

        kubectl rollout history deploy/webapp

    e. update the deployment with the image version latest and check the history
    and verify nothing is going on

    answer:
        kubectl set image deployment webapp nginx=nginx:latest --record=true

        kubectl rollout history deploy webapp 
    
    deployment.apps/webapp 
    REVISION  CHANGE-CAUSE
    2         kubectl1.25.4 set image deployment webapp nginx=nginx:1.17.4 --record=true
    4         kubectl1.25.4 set image deployment webapp nginx=nginx:1.100 --record=true
    5         <none>
    6         kubectl1.25.4 set image deployment webapp nginx=nginx:latest --record=true
    

11. Apply the autoscaling to this deployment with minimum 10 and maximum 20 replicas
        and target CPU of 85% and verify hpa is created and replicas are increased to 10 from 1

    answer:

        kubectl autoscale deploy/webapp --cpu-percent=85 --min=10 --max=20

12. empty question?

13. Clean the cluster by deleting deployment and hpa you just created

    answer:

        kubectl delete deploy webapp

        kubectl delete hpa webapp

14. Create a job and make it run 10 times one after one (run > exit > run >exit ..) using
        the following configuration:
    kubectl create job hello-job --image=busybox --dry-run -o yaml -- echo "Hello I am
    from job" > hello-job.yaml”

    a. Add to the above job completions: 10 inside the yaml
    
    answer: 

         hello-job.yaml