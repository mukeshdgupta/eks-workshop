# eks-workshop

# command to create cluster

eksctl create cluster -f eks-demo-cluster.yaml

kubectl get nodes 


# ADD CONSOLE CREDENTIAL

# Use the command below to define the role ARN(Amazon Resource Number).

rolearn=$(aws cloud9 describe-environment-memberships --environment-id=$C9_PID | jq -r '.memberships[].userArn')

echo ${rolearn}
# [!] If echo command’s result contains assumed-role, perform the additional actions below.

assumedrolename=$(echo ${rolearn} | awk -F/ '{print $(NF-1)}')
rolearn=$(aws iam get-role --role-name ${assumedrolename} --query Role.Arn --output text) 
# Create an identity mapping.

eksctl create iamidentitymapping --cluster eks-demo --arn ${rolearn} --group system:masters --username admin
# You can check aws-auth config map information through the command below.

kubectl describe configmap -n kube-system aws-auth
# When the above operations are completed, you will be able to get information from the control plane, 
# the worker node, logging activation, and update information in Amazon EKS console.
