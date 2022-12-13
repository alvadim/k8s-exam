1. Type the command for: Get pods with label information


        kubectl get pods --show-labels

2. Create 5 nginx pods in which two of them is labeled env=prod and three of them is
labeled env=dev 


        2pod.yaml

        3pod.yaml

3. Verify all the pods are created with correct labels
     

        kubectl get po --show-labels
   
4. Get the pods with label env=dev 


        kubectl get po -l env=dev

5. Get the pods with label env=dev and also output the labels


        kubectl get po -l env=dev --show-labels


6. Get the pods with label env=prod


        kubectl get po -l env=prod

7. Get the pods with label env=prod and also output the labels


        kubectl get po -l env=prod --show-labels

8. Get the pods with label env


        kubectl get po -l env

9. Get the pods with labels env=dev and env=prod


        kubectl get po -l 'env in (dev, prod)'

10. Get the pods with labels env=dev and env=prod and output the labels as well


        kubectl get po -l 'env in (dev, prod)' --show-labels

11. Change the label for one of the pod to env=uat and list all the pods to verify


        kubectl label pods 3pod-deployment-6dd45d79c4-98qdz env=uat --overwrite

        kubectl get po --show-labels

12. Remove the labels for the pods that we created now and verify all the labels are
      removed

        kubectl label pods --all env-

13. Let’s add the label app=nginx for all the pods and verify (using kubectl)


        kubectl label pods --all app=nginx

14. Get all the nodes with labels (if using minikube you would get only master node)


        kubectl get nodes --show-labels

15. Label the worker node nodeName=nginxnode


        kubectl label nodes worker nodeName=nginxnode

16. Create a Pod that will be deployed on the worker node with the label nodeName=nginxnode

          
        see nginx-pod.yaml

17. Verify the pod that it is scheduled with the node selector on the right node… fix it if
       it’s not behind scheduled.

        kubectl describe pods nginx | grep -i node 

         -- pod runs on node with nodeName=nginxnode label

18. Verify the pod nginx that we just created has this label


        kubectl describe pods nginx | grep node 