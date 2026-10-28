FROM python:3.13.18-slim

WORKDIR /app

# Install dependencies first so this layer is cached between builds
COPY requirements.txt .
# pip and setuptools are not needed at runtime and carry vendored CVEs
RUN pip install --no-cache-dir -r requirements.txt \
 && pip uninstall -y pip setuptools

COPY app.py .

# Run as an unprivileged user instead of root
RUN useradd --create-home --uid 10001 appuser
USER appuser

EXPOSE 5000
CMD ["python", "app.py"]
