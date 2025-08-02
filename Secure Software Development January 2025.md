# Secure Software Development January 2025

#### Unit 5: Future Trends in Secure Software Development of the module: Moving Beyond Microservices

## Introduction
Building on the microservices paradigm, modern software development has shifted towards architectures that streamline deployment, enhance scalability, and embed security throughout the development lifecycle (Lewis and Fowler, 2014). This shift is driven by the rising complexity of distributed systems, the need for rapid feature releases, and an increasingly significant focus on secure software practices (Souppaya and Scarfone, 2016). Containers, serverless computing, and service meshes represent key advancements that extend beyond traditional microservices, enabling teams to concentrate on application logic rather than infrastructure management.

### Containerisation and Orchestration
Containers, popularised by technologies such as Docker, encapsulate applications and their dependencies to maintain consistent environments across development, testing, and production (Pahl, 2015). Coupled with orchestration platforms like Kubernetes, containers can be automatically scaled, monitored, and updated, resulting in greater operational efficiency and reliability (Kubernetes, 2023).

### Serverless and Function-as-a-Service (FaaS)
Serverless computing, often delivered via Function-as-a-Service (FaaS), reduces infrastructure overhead by allowing developers to deploy discrete functions that run solely in response to events or triggers (AWS, 2023). This approach offers cost-effectiveness—billing only for actual execution—and expedites feature delivery by removing the need for teams to manage the underlying infrastructure (Eivy, 2017).

### Service Mesh and Observability
Service meshes, such as Istio, add a dedicated infrastructure layer to manage service-to-service communication, enforce security policies, and collect telemetry data (IBM, 2022). Coupled with observability tools (e.g., Prometheus and Grafana), they enable teams to maintain detailed insights into distributed systems, aiding in both proactive performance management and rapid incident response (NIST, 2019).

## Conclusion
Modern software development continues to evolve beyond microservices to embrace container orchestration, serverless computing, and advanced service mesh solutions. These innovations align with industry-wide demands for agility, scalability, and integrated security strategies. Forward-thinking organisations must adapt to these paradigms to ensure efficient workflows and robust security postures in an era of increasingly complex distributed environments.

---

# Formative Activity: RESTful API Creation

This section aligns with the following learning outcomes:

1. **Critically analyse development problems** and determine appropriate methodologies, tools, and techniques for programming solutions.  
2. **Design and develop computer programs** to meet specified briefs, and critically evaluate resulting solutions.  
3. **Implement and refine team skills** in a virtual, professional environment, reflecting real-life organisational roles and structures.

Below are summarised answers to the formative activities based on the `api.py` script (which uses Flask and Flask-RESTful to create a basic RESTful API).

## API Code

```python
from flask import Flask
from flask_restful import Api, Resource, reqparse
 
app = Flask(__name__)
api = Api(app)
 
users = [
    {
        "name": "James",
        "age": 30,
        "occupation": "Network Engineer"
    },
    {
        "name": "Ann",
        "age": 32,
        "occupation": "Doctor"
    },
    {
        "name": "Jason",
        "age": 22,
        "occupation": "Web Developer"
    }
]
 
class User(Resource):
    def get(self, name):
        for user in users:
            if(name == user["name"]):
                return user, 200
        return "User not found", 404
 
    def post(self, name):
        parser = reqparse.RequestParser()
        parser.add_argument("age")
        parser.add_argument("occupation")
        args = parser.parse_args()
 
        for user in users:
            if(name == user["name"]):
                return "User with name {} already exists".format(name), 400
 
        user = {
            "name": name,
            "age": args["age"],
            "occupation": args["occupation"]
        }
        users.append(user)
        return user, 201
 
    def put(self, name):
        parser = reqparse.RequestParser()
        parser.add_argument("age")
        parser.add_argument("occupation")
        args = parser.parse_args()
 
        for user in users:
            if(name == user["name"]):
                user["age"] = args["age"]
                user["occupation"] = args["occupation"]
                return user, 200
        
        user = {
            "name": name,
            "age": args["age"],
            "occupation": args["occupation"]
        }
        users.append(user)
        return user, 201
 
    def delete(self, name):
        global users
        users = [user for user in users if user["name"] != name]
        return "{} is deleted.".format(name), 200
      
api.add_resource(User, "/user/<string:name>")
 
app.run(debug=True)
```