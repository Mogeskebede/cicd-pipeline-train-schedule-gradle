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

**Subject:** Request for Clarification on Data Center Isolation and Alerts for AIT-72719  

Dear Team,  

We would like to inform you that AIT-72719 has been fully onboarded to the OpenShift Container Platform. However, we recently learned about a planned data center isolation for *iseast26*. Our services are currently running in *useast26* and *uscentral12* within the Production environment.  

Unfortunately, the CAPI team was not notified of this upcoming event. This raises concerns about whether our AIT is accurately reflected as fully onboarded in your system.  

We kindly request your guidance on the following:  
1. **Alerting Mechanism**: How can we ensure that we receive timely notifications about such events in the future?  
2. **Impact Assessment**: Could you please clarify the potential impact of the *iseast26* data center isolation on our services?  

Your prompt response will help us take the necessary actions to minimize any disruptions.  

Thank you for your attention to this matter.  

Best regards,  
[Your Name]  
CAPI Team  




It is not necessary to run this app locally in order to complete the learning activities, but if you wish to do so you will need a local installation of npm. Begin by installing the npm dependencies with:

    npm install

Then, you can run the app with:

    npm start

Once it is running, you can access it in a browser at [http://localhost:3000](http://localhost:3000)
