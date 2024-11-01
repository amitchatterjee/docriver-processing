#######################
# One-time setup
#######################
pip install apache-airflow

# Add to ~/.bashrc
# Change the line below to point to the root of the docriver source 
export DOCRIVER_PROC_HOME=$HOME/git/docriver-processing
# Make changes to the env.sh file as needed
source $DOCRIVER_PROC_HOME/env.sh
source ~/docriver-venv/bin/activate
# Exit the shell and create a new one before continuing further

#######################
# Operations
#######################
# Start backend components, if it has not been started already
docker compose -f $DOCRIVER_GW_HOME/infrastructure/compose/docker-compose-backend.yml -p docriver up -d

# Start airflow
docker compose -f $DOCRIVER_PROC_HOME/infrastructure/compose/docker-compose-airflow.yml -p docriver up -d

# Run command from CLI
docker compose -f $DOCRIVER_PROC_HOME/infrastructure/compose/docker-compose-airflow.yml -p docriver run --rm  airflow-cli airflow dags  backfill hello-world --start-date 2015-06-01 --end-date 2015-06-07