#######################
# One-time setup
#######################
# Create python venv - because it runs on python 3.8, not 3.13 like the rest of the docriver. This is not ideal
cd $HOME
python -m venv airflow-venv

source ~/airflow-venv/bin/activate
pip install apache-airflow

# Add to ~/.bashrc
# Change the line below to point to the root of the docriver source 
export DOCRIVER_PROC_HOME=$HOME/git/docriver-processing
# Make changes to the env.sh file as needed
source $DOCRIVER_PROC_HOME/env.sh

# Exit the shell and create a new one before continuing further

#######################
# Operations
#######################
# Start backend components, if it has not been started already
docker compose -f $DOCRIVER_GW_HOME/infrastructure/compose/docker-compose-backend.yml -p docriver up -d

# Start airflow
docker compose -f $DOCRIVER_PROC_HOME/infrastructure/compose/docker-compose-airflow.yml -p docriver up -d

# Access console
http://localhost:8081
docriver/docriver

# Run command from CLI
docker compose -f $DOCRIVER_PROC_HOME/infrastructure/compose/docker-compose-airflow.yml -p docriver run --rm  airflow-cli airflow dags  backfill hello-world --start-date 2015-06-01 --end-date 2015-06-07