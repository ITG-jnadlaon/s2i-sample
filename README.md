# s2i-sample

# Login to OpenShift
oc login <your-openshift-cluster>

# Create new project
oc new-project s2i-mongodb-demo

# Deploy MongoDB with persistent storage
oc new-app mongodb-persistent \
  -p MONGODB_USER=appuser \
  -p MONGODB_PASSWORD=secretpass123 \
  -p MONGODB_DATABASE=myapp \
  -p MONGODB_ADMIN_PASSWORD=adminpass123

# Deploy Node.js application using S2I
oc new-app nodejs:18-ubi8~https://github.com/ITG-jnadlaon/s2i-sample \
  --name=s2i-mongodb-app \
  -e MONGODB_URI=mongodb://appuser:secretpass123@mongodb:27017/myapp \
  -e MONGODB_DATABASE=myapp

# Expose the application
oc expose svc/s2i-mongodb-app

# Check the route
oc get route s2i-mongodb-app
