currently i am working in Risk Work station which have huge number of modules in that I works for Limit Excess service(Limit Exception System),
which interacts with other micro services. in those mainly
ewi microservice, workflow services which internally contains excess and bpm services.
and right now excess service extraction from workflow services is in progress.
before that for the last 2+ years i worked in this project mainly in UI and front end service that is inbox module which contains inbox UI and service.

let me explain briefly about the basic functionality of our module. and even which contains the technical stack of our module.
let me explain briefly about the techinical stack along with basic functionality of our module.

1. first ewi service utilises the LMS (Limit management system) data and generates alerts and pushes the required data to kafka.
2. and the kafka consumer is cofigured in the workflow services which will cosume the alert messages and creates payload in BPM by using excess service apis.
3. after the payload data generated in the bpm useually which will be user sepecific. <-- JAVA 8 and Oracle DB, Spring Boot, BPM
4. when the user login to the RWS application and clicks inbox tab we will populate the users specific data by hitting api service of inbox service module
   and from there it will call the workflow services api to get that user's specific data available in the BPM.

Here my role is individual contributer which contains 90% of development and 10% of assistance to junior and other coleagues.
coming to work which i have done so far, mainly compraises of

1. (initially worked on) improvement of some screens look and feel.
2. later involved in services like export to PDF and Excel, email file generation.
3. and some performance improvement tasks in UI and java service.
4. And recently involved in aws migration of one module.

Why you want to move?
For the last two years, i have been involved in multiple modules in RWS project.
but with the same kind of work. I have continued for 2 years because there is a change in the module.
but now I will be getting same work from the old modules.
some how feel exausted and stuck in the same project. I want to explore new challenges and make my self available to learn new things
and hence i am looking for a change.

Micro Services with Circuit Breaker and Kafka

Involved in the Software Development Life Cycle phases which include Analysis, Design, Implementation, Testing and Maintenance.

Worked on various projects using methodologies like SDLC, Agile and Dev Ops
