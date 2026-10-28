FROM python:3.13.18-slim

WORKDIR /app

# Install dependencies first so this layer is cached between builds
COPY requirements.txt .
# pip and setuptools are not needed at runtime and carry vendored CVEs
RUN pip install --no-cache-dir -r requirements.txt \
 && pip uninstall -y pip setuptools

COPY app.py .

# Version shown on the page, set at build time: --build-arg APP_VERSION=1.0.0
ARG APP_VERSION=dev
ENV APP_VERSION=${APP_VERSION}

# Run as an unprivileged user instead of root
RUN useradd --create-home --uid 10001 appuser
USER appuser

EXPOSE 5000
CMD ["python", "app.py"]
