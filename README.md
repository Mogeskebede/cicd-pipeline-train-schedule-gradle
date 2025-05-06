# cicd-pipeline-train-schedule-apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: buildconfigs.build.openshift.io
spec:
  group: build.openshift.io
  names:
    kind: BuildConfig
    listKind: BuildConfigList
    plural: buildconfigs
    singular: buildconfig
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
          status:
            type: object


This is a simple train schedule app written using nodejs. It is intended to be used as a sample application for a series of hands-on learning activities.

## Running the app


Here’s an advanced version of your email with improved tone and structure:  

---

Steps to Include Route Hostnames in the Ingress Gateway
Specify the Hostnames in the Ingress Gateway Listener
In Consul's IngressGateway configuration, you can define the hostnames for each service. This ensures that the gateway routes incoming requests to the appropriate service based on the requested hostname.
Example:

apiVersion: consul.hashicorp.com/v1alpha1
kind: IngressGateway
metadata:
  name: ingress-gateway
spec:
  listeners:
    - port: 443
      protocol: http
      services:
        - name: service-a
          hosts:
            - service-a.company.com
        - name: service-b
          hosts:
            - service-b.company.com
Ensure Route Configuration Matches the Ingress Gateway
Update your OpenShift routes to use the same hostnames. For example:
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: service-a-route
spec:
  host: service-a.company.com
  to:
    kind: Service
    name: service-a
  tls:
    termination: passthrough
    insecureEdgeTerminationPolicy: Redirect
TLS Certificate for Hostnames
If your ingress gateway serves HTTPS traffic, ensure the certificate includes all the hostnames in the Subject Alternative Name (SAN) field. Use a wildcard certificate or include all hostnames explicitly when issuing the certificate.
HAProxy Configuration
If HAProxy is the ingress controller, it should also handle routing based on these hostnames. Ensure the HAProxy frontend configuration includes host-based routing:
frontend https_frontend
    bind *:443 ssl crt /etc/ssl/certs/company.pem
    acl host_service_a hdr(host) -i service-a.company.com
    acl host_service_b hdr(host) -i service-b.company.com
    use_backend service_a_backend if host_service_a
    use_backend service_b_backend if host_service_b
Why Include Route Hostnames?
It ensures proper routing of traffic to the correct microservice.
It allows hostname-based security policies, including SSL/TLS and access control.
It aligns your application architecture for external access requirements.
By including the route hostnames in the Ingress Gateway configuration, you create a seamless and secure flow of traffic that matches your company network's security policies and requirements.

You said:
What about the port number. Is it going to be my app port or 8443/433
ChatGPT said:
The port number exposed on the Ingress Gateway will depend on how you configure the gateway to handle incoming traffic and forward it to your application.

Key Points for Port Configuration
Ingress Gateway Port:
The Ingress Gateway exposes a port to the external traffic, such as 443 (for HTTPS) or 8443 (if using a non-standard HTTPS port).
This is the port clients will use to connect to your application via the gateway.
Application Service Port:
Your application services typically listen on their internal ports (e.g., 8080 or 5000).
The Ingress Gateway will forward traffic from its external port (e.g., 443) to the application's internal service port.
Configuration Example
Ingress Gateway Configuration

In the Ingress Gateway, you configure the external port and the backend (application) port. For example:

apiVersion: consul.hashicorp.com/v1alpha1
kind: IngressGateway
metadata:
  name: ingress-gateway
spec:
  listeners:
    - port: 443
      protocol: http
      services:
        - name: service-a
          hosts:
            - service-a.company.com
          port: 8080  # Internal application port
Here:

External traffic comes to port 443 on the gateway.
The gateway forwards traffic to the application listening on port 8080.
OpenShift Route

The route should use passthrough termination, as the TLS is terminated at the Ingress Gateway, not the route:

apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: service-a-route
spec:
  host: service-a.company.com
  to:
    kind: Service
    name: service-a
  tls:
    termination: passthrough
    insecureEdgeTerminationPolicy: Redirect
HAProxy Configuration

If you're using HAProxy as the ingress controller, ensure that:

HAProxy listens on port 443 (or 8443 if non-standard HTTPS is used).
HAProxy forwards traffic to the application port.
Example:

frontend https_frontend
    bind *:443 ssl crt /etc/ssl/certs/company.pem
    acl host_service_a hdr(host) -i service-a.company.com
    use_backend service_a_backend if host_service_a

backend service_a_backend
    server service-a <service-a-ip>:8080 check ssl verify none
Which Port Should You Use?
External Clients:
Use the port exposed by the Ingress Gateway or HAProxy (443 or 8443).
Example: https://service-a.company.com uses port 443.
Internal Forwarding:
Use the application's internal service port (e.g., 8080 or 5000).
This is configured in the gateway or HAProxy backend settings.
By following this setup, external clients access your app via 443 (or 8443), while the gateway forwards traffic to your application's internal






 

  




It is not necessary to run this app locally in order to complete the learning activities, but if you wish to do so you will need a local installation of npm. Begin by installing the npm dependencies with:

    npm install

Then, you can run the app with:

    npm start

Once it is running, you can access it in a browser at [http://localhost:3000](http://localhost:3000)
