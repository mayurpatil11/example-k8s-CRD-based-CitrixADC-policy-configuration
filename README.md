# Citrix ADC Policy Configuration automation using Kubernetes CRDs

Citrix provides Kubernetes CustomResourceDefinitions (CRDs) that you can use in conjunction with Citrix Ingress Controller (CIC) and automate the configurations and deployment of these policies on the Citrix ADCs used as Ingress devices


We are going to use Citrix ADC two tier hairpin model deployment for configuring rewrite and responder policy in Tier 1 ADC.
Follow the below guide for expericing automatic deployment of Citrix ADC policies.

1. Deploy the Citrix ADC in two tier microservice deployement
Please refer to https://github.com/citrix/example-cpx-vpx-for-kubernetes-2-tier-microservices guide for deploying the Citrix ADC in kubernetes cluster. 

2. Validate step 1 by accessing the microservice applications and copy the yaml files from ``/CRD-yaml-files/`` to ``/root/yamls/`` folder in your local machine
e.g. ``https://hotdrink.beverages.com``


![hotbeverage_webpage](https://user-images.githubusercontent.com/42699135/50677394-987efb00-101f-11e9-87d1-6523b7fbe95a.png)

3. Deploy the Citrix CRD 
Rewrite and Responder CRD in the Kubernetes cluster enables you to configure extensive rewrite and responder policies in Tier 1 ADC.
```
kubectl create -f /root/yamls/CRD_rewrite-responder-policies-deployment.yaml
```

4. Configure Responder policy on Tier 1 ADC (VPX) to block the user access to coffee beverage microservice
```
kubectl create -f /root/yamls/responder_blockURL.yaml -n tier-2-adc
```
After successful deployment of responder policy yaml file, try accessing coffee beverage php webpage and You will see that access is blocked.
e.g. Try accessing ``https://hotdrink.beverages.com/coffee.php`` and see the response as shown in below figure;

![blockpage](https://user-images.githubusercontent.com/42699135/53745271-b4d6d100-3ec4-11e9-8c6b-e1ff79638b41.png)

5. Configure Rewrite policy on on Tier 1 ADC (VPX) to insert custom header in response header list
```
kubectl create -f /root/yamls/rewritePolicy-insertHeader.yaml -n tier-2-adc
```
Try accessing ``https://colddrink.beverages.com`` and you will find sessionID header being inserted in HTTP response header list.

![rewritepage](https://user-images.githubusercontent.com/42699135/53745280-b902ee80-3ec4-11e9-9f6e-d44bb663cf9b.png)

To know more about other responder and rewrite policy configuration, please refer to:- https://github.com/citrix/citrix-k8s-ingress-controller/blob/master/docs/rewrite-responder-policy.md

