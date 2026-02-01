# GitLab CI/CD Project – Node.js Docker App

This project demonstrates a **full GitLab CI/CD pipeline** for a Node.js Docker application, including build, test, push, and deploy stages.

---

## 1. Pipeline Stages

We define four stages in the pipeline:

```yaml
stages:
  - build
  - test
  - push
  - deploy
```

---

## 2. Jobs Definition

### 2.1 Build Job

* Builds the Docker image for the Node.js app.
* Uses a **tagged runner** `dev-env`.

```yaml
Build_job:
  stage: build
  script:
    - docker build -t node-app:latest .
  tags:
    - dev-env
```

---

### 2.2 Test Job

* Placeholder for testing logic.
* Can be extended with real unit or integration tests.
* Runs in parallel if multiple test jobs are added.

```yaml
Test_job:
  stage: test
  script:
    - echo "Running tests..."
  tags:
    - dev-env
```

---

### 2.3 Push Job

* Pushes the Docker image to Docker Hub.
* Uses **GitLab CI/CD variables** for credentials instead of hardcoding them.

```yaml
Push_job:
  stage: push
  script:
    - docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS
    - docker tag node-app:latest $DOCKERHUB_USER/node-app:latest
    - docker push $DOCKERHUB_USER/node-app:latest
  tags:
    - dev-env
```

> **Note:** `$DOCKERHUB_USER` and `$DOCKERHUB_PASS` should be stored securely in **GitLab → Settings → CI/CD → Variables**.

---

### 2.4 Deploy Job

* Deploys the application using **docker-compose**.
* Can be extended to include **health checks** or **rollback logic**.

```yaml
Deploy_job:
  stage: deploy
  script:
    - docker-compose up -d
  tags:
    - dev-env
```

---

## 3. Best Practices

1. **Use variables for sensitive information**

   * Avoid hardcoding credentials in `.gitlab-ci.yml`.
   * Example: `$DOCKERHUB_USER`, `$DOCKERHUB_PASS`.

2. **Use tags for runner selection**

   * Ensure jobs run on the intended runner (e.g., `dev-env`).

3. **Add testing and scanning jobs**

   * Use static code analysis or security scanning in the `test` stage.

4. **Artifacts and logs**

   * Store logs or reports as artifacts for later inspection.

```yaml
artifacts:
  paths:
    - logs/
  expire_in: 1 week
```

5. **Parallel Jobs**

   * Multiple test jobs can run in parallel to speed up the pipeline.

---


```
stages:
  - build
  - test
  - push
  - deploy


Build_job:
  stage: build
  script:
    - docker build -t node-app:latest .
  tags:
    - dev-env

Test_job:
  stage: test
  script:
    - echo "Testing ......"
  tags:
    - dev-env

Push_job:
  stage: push
  script:
    - docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS
    - docker image tag node-app:latest $DOCKERHUB_USER/node-app
    - docker push $DOCKERHUB_USER/node-app:latest
  tags:
    - dev-env

Deploy_job:
  stage: deploy
  script:
    - docker-compose up -d
  tags:
    - dev-env
```
