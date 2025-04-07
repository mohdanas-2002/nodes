Objective :
Automate the process of testing, building, and deploying a Node.js web app with GitHub Actions. Use DockerHub for deploying Docker images.

Pre-requisites :
 Create a GitHub repository and push your Node.js app code there.
 Ensure you have a DockerHub account to push your Docker images.
 A sample app to demonstrate the process.
 Create a  at the root of your project to containerize the application.


Steps to Set Up the CI/CD Pipeline :

Create a Workflow File :
 Inside your GitHub repository, navigate to main.yml .
 Create a file named main.yml : 

        name: CI/CD Pipeline

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Log in to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push Docker image
      run: |
        docker build -t amanuddinu4/nodejs-demo-app .
        docker push amanuddinu4/nodejs-demo-app

Prepare Your Dockerfile : 

FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]

Configure GitHub Secrets :
Go to your GitHub repository settings → Secrets and variables → Actions
Add the following secrets:
 DOCKERHUB_TOKEN : DockerHub access token.  
 DOCKERHUB_USERNAME : Your DockerHub username. 

Test Your Pipeline :
Commit and push your changes to the  branch.
GitHub Actions will automatically trigger the workflow on every push to main.


