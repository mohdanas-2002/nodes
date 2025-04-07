Objective :<br>
Automate the process of testing, building, and deploying a Node.js web app with GitHub Actions. Use DockerHub for deploying Docker images.<br>

Pre-requisites :<br>
 Create a GitHub repository and push your Node.js app code there.<br>
 Ensure you have a DockerHub account to push your Docker images.<br>
 A sample app to demonstrate the process.<br>
 Create a  at the root of your project to containerize the application.<br>


Steps to Set Up the CI/CD Pipeline :<br>

Create a Workflow File :<br>
 Inside your GitHub repository, navigate to main.yml .<br>
 Create a file named main.yml : <br>

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
        <br>

Prepare Your Dockerfile :<br> 

FROM node:18<br>
WORKDIR /app<br>
COPY . .<br>
RUN npm install<br>
EXPOSE 3000<br>
CMD ["npm", "start"]<br>
<br>
Configure GitHub Secrets :<br>
Go to your GitHub repository settings → Secrets and variables → Actions<br>
Add the following secrets:<br>
 DOCKERHUB_TOKEN : DockerHub access token. <br> 
 DOCKERHUB_USERNAME : Your DockerHub username. <br>

Test Your Pipeline :<br>
Commit and push your changes to the  branch.<br>
GitHub Actions will automatically trigger the workflow on every push to main.<br>


