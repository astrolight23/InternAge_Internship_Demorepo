# InternAge_Internship_Demorepo

# 🚀 Deploy HTML Site to GHCR Using GitHub Actions

Perfect 👍 Let’s deploy your simple HTML site to **GHCR** automatically using **GitHub Actions**.

This will:

- ✅ Build the Docker image  
- ✅ Push it to **ghcr.io**  
- ✅ Run automatically on every push to `main`

---

# 1️⃣ Project Structure

Your repo should look like this:

```

.
├── index.html
├── Dockerfile
└── .github/
└── workflows/
└── docker.yml

````

---

# 2️⃣ Your `index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Simple Site</title>
</head>
<body>
  <h1>Hello from GHCR 🚀</h1>
</body>
</html>
````

---

# 3️⃣ Your Dockerfile

```dockerfile
FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
COPY index.html /usr/share/nginx/html/
EXPOSE 80
```

---

# 4️⃣ Create GitHub Actions Workflow

Create this file:

```
.github/workflows/docker.yml
```

### `docker.yml`

```yaml
name: Build and Push to GHCR

on:
  push:
    branches:
      - main

permissions:
  contents: read
  packages: write

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Log in to GHCR
        run: echo "${{ secrets.GITHUB_TOKEN }}" | docker login ghcr.io -u ${{ github.actor }} --password-stdin

      - name: Build Docker image
        run: |
          docker build -t ghcr.io/${{ github.repository }}:latest .

      - name: Push Docker image
        run: |
          docker push ghcr.io/${{ github.repository }}:latest
```

---

# 5️⃣ How It Works

* `GITHUB_TOKEN` is automatically provided by GitHub
* No need to create a Personal Access Token
* Image name becomes:

```
ghcr.io/OWNER/REPO:latest
```

Example:

```
ghcr.io/john/my-simple-site:latest
```

---

# 6️⃣ After Pushing

1. Push your code to `main`
2. Go to:

   * Your repo → **Actions** tab
3. You’ll see the workflow running
4. After success:

   * Go to your GitHub profile → **Packages**
   * Your container will be there 🎉

---

# 7️⃣ Make It Public (Optional)

1. Profile → Packages
2. Select your container
3. Change visibility to **Public**

---

# 🚀 Run the Image Anywhere

```bash
docker run -p 8080:80 ghcr.io/OWNER/REPO:latest
```

---

## 🔥 Next Steps

You can enhance this setup with:

* 🔥 Auto-version tagging (`v1.0.0` instead of `latest`)
* 🔥 Multi-arch builds (amd64 + arm64)
* 🔥 Automatic VPS deployment after push
* 🔥 Kubernetes deployment pipeline

```

---

If you'd like, I can also generate:

- 📄 A production-ready README.md version  
- 🏢 Enterprise-grade CI/CD markdown documentation  
- 📦 A full DevOps project template structure  

Just tell me the level you want.
```
