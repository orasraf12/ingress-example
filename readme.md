"""
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
"""
