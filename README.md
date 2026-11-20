# DevOps Practical — Complete Flask Application

This source code implements the final stage of the uploaded DevOps Practical: a Flask app, pytest test, GitHub Actions CI, Docker image recipe, and Kubernetes Deployment/Service.

## Files
- `app.py`: Final V2 app with Render-compatible `PORT` environment variable.
- `test_app.py`: Automated test matching final V2 message (keeps GitHub Actions green).
- `requirements.txt`: Exact dependency versions from the worksheet.
- `.github/workflows/ci.yml`: Run automated tests on pushes to `main`.
- `Dockerfile`: Package app as a container.
- `k8s/deployment.yaml`: Two replicas; **replace `YOUR-DOCKERHUB-USERNAME`**.
- `k8s/service.yaml`: NodePort service.

## Local commands (Windows PowerShell)
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m pytest -q
python app.py
```
Open http://localhost:5000 after running the app.

## Docker
```powershell
docker build -t flask-app:v1 .
docker run --rm -p 5000:5000 flask-app:v1
```

## GitHub
Create an empty `devops-practical` repository, then run:
```powershell
git init
git add .
git commit -m "Initial commit: DevOps practical"
git branch -M main
git remote add origin https://github.com/YOUR-GITHUB-USERNAME/devops-practical.git
git push -u origin main
```
Use your actual GitHub username (the worksheet has an inconsistent `devops-masterclass.git` URL; the repository should consistently be `devops-practical`).

## Docker Hub / Kubernetes
```powershell
docker login
docker tag flask-app:v1 YOUR-DOCKERHUB-USERNAME/flask-app:v1
docker push YOUR-DOCKERHUB-USERNAME/flask-app:v1
minikube start --driver=docker
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get pods
minikube service flask-service
```
Before `kubectl apply`, update `k8s/deployment.yaml` with your Docker Hub username. To demonstrate self-healing, delete one pod with `kubectl delete pod ACTUAL-POD-NAME` then check `kubectl get pods` again.

## Render cloud deployment
Create a Render Web Service connected to the GitHub repository, using `main` and Docker. When the deployment succeeds, note the real Render URL. A subsequent commit changing the app and its expected test response should automatically redeploy the live web app. See the separate Execution and Submission Guide for the exact order and screenshots.

## Important
The source files can be prepared offline, but GitHub push, CI completion on GitHub, Docker Hub push, Minikube operation, and Render deployment **must be performed in your own accounts/local environment**. No screenshots or successful external deployment results are claimed by this package.
