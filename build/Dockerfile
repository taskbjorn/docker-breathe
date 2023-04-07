# Build on the stable NGINX Docker image.
FROM nginx:1

# Install required packages through APT.
# This step installs Git and Python, as well as Doxygen,Sphinx and Breathe for
# documentation generation.
RUN apt-get update && \
    apt-get install -y \
        doxygen \
        git \
        python3 \
        python3-breathe \
        python3-pip \
        python3-sphinx && \
    apt-get clean

# Install required Python packages.
# This step installs Sphinx extensions and themes and ensure the latest version
# at the time of build is installed.
RUN pip install \
        pydata-sphinx-theme

# Configure SSH keys and working directory.
# This step launches the SSH agent and adds any keys in the ~/.ssh folder on
# start. Moreover, it disables host key checking to avoid any user-prompts
# during connection. Finally, it creates an empty directory for the cloned
# repository.
RUN echo "eval $(ssh-agent)\nssh-add /root/.ssh/*" >> /root/.profile && \
        mkdir -p /root/.ssh && \
        echo "Host *\n\tStrictHostKeyChecking no" >> /root/.ssh/config && \
        chmod 700 /root/.ssh && \
        chmod 600 /root/.ssh/config && \
        mkdir -p /repo

# Set the working directory.
# This steps sets the working directory to a known path where the repository
# will be cloned to.
WORKDIR /repo

# Set the default shell to Bash.
# Optional step, I prefer to use Bash over sh when available.
SHELL ["/bin/bash", "-c"]

# Copy the entrypoint script.
COPY entrypoint.py /

# Launch the entrypoint script on startup.
CMD ["python3", "/entrypoint.py"]