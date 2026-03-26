FROM python:3.11-slim

# Install system dependencies + R
RUN apt-get update && apt-get install -y \
    r-base \
    r-base-dev \
    libcurl4-openssl-dev \
    libssl-dev \
    libxml2-dev \
    && rm -rf /var/lib/apt/lists/*

# Install JupyterLab
RUN pip install --no-cache-dir jupyterlab

# Install IRkernel in R
RUN R -e "install.packages('IRkernel', repos='https://cloud.r-project.org'); \
          IRkernel::installspec(user = FALSE)"

# Expose Jupyter port
EXPOSE 8888

# Start JupyterLab
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root"]
