# Django Vue Integration Project

This project is a full-stack web application integrating Django and Vue.js. The backend is powered by Django, while the frontend is built using Vue.js.

## Prerequisites

Make sure you have the following installed:

- Anaconda 3
- Python 3.11
- Django 4.2
- Django rest framework
- Node.js 14+
- npm 6+
- Vue CLI

### Backend Setup (Django)

1. **Clone the repository**

    ```bash
    git clone https://github.com/your-repo-url.git
    cd your-repo-folder
    ```

2. **Create a virtual environment**

    ```bash
    conda create --name environmentName python=3.11
    conda activate environmentName
    ```

3. **Install dependencies**

    ```bash
    pip install -r requirements.txt
    ```

4. **Set environment variables (.env)**

    ```bash
    DJANGO_SECRET_KEY=your_secret_key
    ```

5. **Migrate the database**

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

6. **Start the server**

    ```bash
    python manage.py runserver
    ```


### Frontend Setup (Vue)
**Project setup**
```
npm install
```

**Compiles and hot-reloads for development**
```
npm run serve
```

**Compiles and minifies for production**
```
npm run build
```

**Reference Data**
See [E-commerce Website With Django and Vue Tutorial (Django Rest Framework)](https://www.youtube.com/watch?v=Yg5zkd9nm6w)
See [Configuration Reference](https://cli.vuejs.org/config/).
