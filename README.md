
on:
push:
branches:
- main
workflow_dispatch:

jobs:
deploy:
runs-on: ubuntu-latest

steps:
- name: Checkout code
  uses: actions/checkout@v4

- name: Set up Node.js
  uses: actions/setup-node@v3
  with:
    node-version: 16

- name: Install dependencies
  run: npm install

- name: Build project
  run: npm run build

- name: Deploy to server
  env:
    DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
    SERVER_IP: ${{ secrets.SERVER_IP }}
    SERVER_USER: ${{ secrets.SERVER_USER }}
  run: |
    ssh -o StrictHostKeyChecking=no -i $DEPLOY_KEY $SERVER_USER@$SERVER_IP "mkdir -p ~/project"
    scp -o StrictHostKeyChecking=no -i 