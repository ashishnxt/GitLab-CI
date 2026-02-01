#GitLab CI/CD Project – Node.js Docker App


This project demonstrates a **full GitLab CI/CD pipeline** for a Node.js Docker application, including build, test, push, and deploy stages.

---

<img width="3194" height="1999" alt="Screenshot 2026-01-26 172430" src="https://github.com/user-attachments/assets/6924d4e7-ec75-4a42-849b-9525253bcdc0" />

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

<img width="3199" height="1999" alt="Screenshot 2026-01-31 210458" src="https://github.com/user-attachments/assets/b9b6df09-877a-409a-80d1-73e689ececc4" />

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

<img width="3194" height="1999" alt="Screenshot 2026-01-31 205805" src="https://github.com/user-attachments/assets/4ed4ac7f-64c1-432d-9582-fef5ce6f8d8d" />

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

<img width="3199" height="1999" alt="Screenshot 2026-01-31 205107" src="https://github.com/user-attachments/assets/1691744b-186a-40f8-beeb-d2322467519c" />

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

<img width="3199" height="1999" alt="Screenshot 2026-02-01 184844" src="https://github.com/user-attachments/assets/a87be017-438b-4adb-9242-582fbbe1f770" />

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

<img width="3194" height="1994" alt="Screenshot 2026-01-31 205843" src="https://github.com/user-attachments/assets/c2123ca7-9898-48c6-ace2-81febe8199e4" />

5. **Parallel Jobs**

   * Multiple test jobs can run in parallel to speed up the pipeline.

---

<img width="3192" height="1985" alt="Screenshot 2026-01-31 210734" src="https://github.com/user-attachments/assets/44dcecb3-3095-4746-bb4a-3cda951e5413" />

<img width="3192" height="1992" alt="Screenshot 2026-01-31 210744" src="https://github.com/user-attachments/assets/392d9e62-c811-4d0b-b519-fcaf81890ebd" />

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
