# Example Rule with ACM (using ALB Ingress Controller):


![Alt text](/posts/path/to/img.jpg "Optional title")


adding waf to albc - https://aws.amazon.com/blogs/containers/protecting-your-amazon-eks-web-apps-with-aws-waf/
```
apiVersion: networking.k8s.io/v1 
kind: Ingress
metadata:
  name: my-alb-ingress
  annotations:
    kubernetes.io/ingress.class: "alb"  # Specify ALB ingress controller
    alb.ingress.kubernetes.io/scheme: internet-facing  # Configure for internet traffic
    alb.ingress.kubernetes.io/ingress.secret: "arn:aws:acm:region:account-id:certificate/certificate-id"  # Reference ACM certificate ARN
    alb.ingress.kubernetes.io/shield-advanced-protection: 'true' #enable shiled in the alb
    alb.ingress.kubernetes.io/wafv2-acl-arn: ${WAF_WACL_ARN} # associate the waf to the alb 
    
spec:
  rules:
    - http:
        paths:
          - path: /path-a/*
            backend:
              serviceName: service-a
              servicePort: 80                        
          - path: /path-b/*
            backend:
              serviceName: service-b
              servicePort: 80            
          - path: /*
            backend:
              serviceName: defualt-service
              servicePort: 80
```


## Breakdown:

1. Ingress Resource: The YAML defines an Ingress resource named my-alb-ingress, specifying that it's managed by the ALB Ingress controller.

2. Scheme: The configuration sets the scheme to internet-facing, indicating that the ALB should be accessible from the internet.

3. TLS Certificate: The alb.ingress.kubernetes.io/ingress.secret annotation references an ACM certificate ARN for secure HTTPS connections.
4. alb.ingress.kubernetes.io/shield-advanced-protection: This annotation enables AWS WAF (Web Application Firewall) & Shield Advanced protection on the Application Load Balancer (ALB). Shield Advanced adds additional DDoS mitigation capabilities on top of the standard Shield protection.

5. alb.ingress.kubernetes.io/wafv2-acl-arn: This annotation associates a WAFv2 Web ACL (Web Application Firewall Access Control List) with the ALB. The Web ACL defines the rules that WAF will use to filter incoming traffic and block potential attacks. You'll need to create the WAFv2 Web ACL separately and provide its ARN (Amazon Resource Name) here.

6. Routing Rules: The rules section defines multiple path-based routing rules:

 * Traffic matching /path-a/* is routed to service-a on port 80.

 * Traffic matching /path-b/* is routed to service-b on port 80.

  * Any remaining traffic (matching /*) is routed to the default-service on port 80, acting as a catch-all rule.

# Key Points:

* This configuration demonstrates how to use the ALB Ingress controller to manage ingress traffic, secure it with TLS, and implement granular path-based routing to different services within the EKS cluster.

* The provided example effectively illustrates the concepts of secure ingress traffic management using the ALB Ingress controller and ACM certificates.
