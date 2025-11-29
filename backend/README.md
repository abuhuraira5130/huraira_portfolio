# myproject (Django + DRF + MySQL)

## Files

- myproject/manage.py
- myproject/myproject/settings.py
- myproject/myproject/urls.py
- myproject/myproject/wsgi.py
- myproject/myproject/asgi.py
- myproject/myproject/templates/redoc.html
- myproject/store/apps.py
- myproject/store/models.py
- myproject/store/serializers.py
- myproject/store/views.py
- myproject/store/urls.py
- myproject/store/admin.py
- myproject/store/management/commands/seed.py
- myproject/store/tests/test_api.py
- myproject/requirements.txt
- myproject/.env.example

## Setup

```bash
python -m venv .venv
. .venv/Scripts/activate  # Windows
pip install -r myproject/requirements.txt
```

### MySQL (create DB and user)

```sql
CREATE DATABASE myproject CHARACTER SET utf8mb4;
CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON myproject.* TO 'myuser'@'%';
FLUSH PRIVILEGES;
```

### Environment

Copy `.env.example` to `myproject/.env` and set values.

### Migrate and run

```bash
python myproject/manage.py makemigrations
python myproject/manage.py migrate
python myproject/manage.py createsuperuser
python myproject/manage.py runserver 0.0.0.0:8000
```

### Seed sample data

```bash
python myproject/manage.py seed
```

## MySQL Workbench

- Host: `127.0.0.1`
- Port: `3306`
- User: `myuser`
- DB: `myproject`
- Open Workbench, create a connection with above values.

## Curl examples

```bash
# Register
curl -X POST http://127.0.0.1:8000/api/auth/register/ -H "Content-Type: application/json" -d '{"username":"u1","password":"pass1234","first_name":"U","last_name":"One","email":"u1@example.com"}'

# Login (Token)
curl -X POST http://127.0.0.1:8000/api/auth/token/ -H "Content-Type: application/json" -d '{"username":"u1","password":"pass1234"}'

# Login (JWT)
curl -X POST http://127.0.0.1:8000/api/auth/jwt/create/ -H "Content-Type: application/json" -d '{"username":"u1","password":"pass1234"}'

# List products
curl http://127.0.0.1:8000/api/products/?search=P&ordering=price&min_price=5

# Create product (admin)
curl -X POST http://127.0.0.1:8000/api/products/ \
  -H "Authorization: Token YOUR_ADMIN_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"X","slug":"x","price":"9.99","inventory":5,"collection":1}'

# Create order (authenticated)
curl -X POST http://127.0.0.1:8000/api/orders/ \
  -H "Authorization: Token YOUR_USER_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"items":[{"product":1,"quantity":2,"unit_price":"9.99"}]}'
```

## OpenAPI

- Schema: `GET /api/schema/`
- Docs: `GET /api/docs/`
- Generate file: `python myproject/manage.py generateschema --format openapi > openapi.json`

## Production checklist

- Set `DEBUG=false`
- Set `ALLOWED_HOSTS`
- `python manage.py migrate`
- `python manage.py collectstatic`

## What next?

1. Add product images upload
2. Implement advanced permissions and rate limiting
3. Deploy to a VPS