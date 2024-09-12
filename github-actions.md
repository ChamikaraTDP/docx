# GitHub Actions

## Notes

- Do not run self hosted runners from public servers
- aws lightsail with 1GB mem is not enough to run self-hosted runner. server just getting crashed.
-


## Resources

### Docs

https://docs.github.com/en/actions/writing-workflows/quickstart




## Useful Items

### Learn YAML 

https://learnxinyminutes.com/docs/yaml/


## Steps to follow

1. Add ruleset to repo
2. Setup environments
3. Install self-hosted runner
4. Configure workflows
5. Test




### Example Workflow

```YAML
name: Deploy to Server
run-name: ${{ github.actor }} is deploying 🚀
on:
  push:
    branches:
      - release
env:
  TARGET_DIR: /home/chamikara/my-apps/github
  REPO_NAME: portfolio

jobs:
  Deploy-to-server:
    runs-on: [self-hosted, linux, x64, aws, light-sail]
    environment: Production
    env:
      TARGET_DIR: ${{ vars.TARGET_DIR }}
      REPO_DIR: ${{ vars.TARGET_DIR }}/portfolio
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by You!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - name: Install dependencies
        run: npm ci
      - run: echo "💡 Copying  secrets."
      - run: mkdir secrets
      - run: touch ./secrets/private.pem
      - run: touch ./secrets/public.pem
      - name: copy pvt key
        env:
          PVT_SCR:  ${{secrets.PRIVATE_KEY}}
        run: echo "$PVT_SCR" >>  ./secrets/private.pem
      - name: copy pub key
        env:
          PUB_SCR: ${{secrets.PUBLIC_KEY}}
        run: echo "$PUB_SCR" >> ./secrets/public.pem
      - name: Update secret files' permissions
        run: chmod a-wx,go-r ./secrets/private.pem ./secrets/public.pem
      - run: echo "💡 Copying envs."
      - run: touch .env.local
      - name: copy env
        env:
          DB_USER: ${{secrets.DB_USER}}
          DB_HOST: ${{vars.DB_HOST}}
          DB_PORT: ${{vars.DB_PORT}}
          DATA_DIR: ${{vars.DATA_DIR}}
        run: echo -en "DB_USER=$DB_USER\nDB_HOST=$DB_HOST\nDB_PORT=$DB_PORT\nDATA_DIR=$DATA_DIR" >> .env.local
      - name: Build for production
        run: npm run build
      - name: Stop pm2 process
        run: pm2 stop ${{ env.REPO_NAME }}
      - name: Remove all files in ${{ env.REPO_DIR }}
        run: rm -r ${{ env.REPO_DIR }}
      - name: Copy new files to ${{ env.REPO_DIR }}
        run: cp -r ${{ github.workspace }} ${{ env.TARGET_DIR }}
      - name: Restart pm2 process
        run: pm2 start ${{ env.REPO_NAME }}
      - name: List files in ${{ env.REPO_DIR }}
        run: |
          ls -a ${{ env.REPO_DIR }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```