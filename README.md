# Jenkins Calculator

A tiny Python project made only to practice Jenkins.

```
jenkins-calculator/
├── app/calculator.py        # the "app": 4 math functions
├── tests/test_calculator.py # the tests Jenkins will run
├── main.py                  # runs the app and prints results
├── requirements.txt         # dependencies (just pytest)
├── pytest.ini               # tells pytest where the code is
├── Jenkinsfile              # the checklist Jenkins follows
└── jenkins-setup/Dockerfile # Jenkins with Python installed
```

## 1. Try it on your computer first (optional)

```bash
python3 -m venv venv
venv/bin/pip install -r requirements.txt
venv/bin/pytest
venv/bin/python main.py
```

## 2. Put the project on GitHub

Create a new **public** repository (e.g. `jenkins-calculator`) and push this folder to it:

```bash
git init
git add .
git commit -m "First version"
git branch -M main
git remote add origin https://github.com/<your-user>/jenkins-calculator.git
git push -u origin main
```

## 3. Start Jenkins with Docker

```bash
docker build -t my-jenkins ./jenkins-setup
docker run -d --name jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home my-jenkins
```

Get the first-login password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Open http://localhost:8080, paste the password, choose **Install suggested plugins**, and create your admin user.

## 4. Create the job

1. **New Item** → name `jenkins-calculator` → choose **Pipeline** → OK
2. Scroll to the **Pipeline** section
3. Definition: **Pipeline script from SCM**
4. SCM: **Git**, Repository URL: your GitHub repo URL
5. Branch Specifier: `*/main`
6. Script Path: `Jenkinsfile`
7. **Save**

## 5. Run it

Click **Build Now**. Open the build and look at the stages: they should all turn green.
Check **Console Output** to see every command Jenkins ran, and **Test Result** to see the 5 tests.

## 6. Break it on purpose

In `app/calculator.py` change `return a + b` into `return a - b`, then commit and push.
Click **Build Now** again: the **Test** stage turns red, **Run the app** is skipped,
and the test report shows exactly which test failed. Fix it, push, build: green again.

> Note: Jenkins runs on `localhost`, so GitHub can't notify it automatically (webhooks need a public address).
> For now start builds with **Build Now**, or enable **Poll SCM** with schedule `H/2 * * * *`
> so Jenkins checks GitHub for new commits every 2 minutes.
