FROM mysql:8.0.33
LABEL "Project"="Containerization"
LABEL "Author"="Ravi"

ADD db_backup.sql docker-entrypoint-initdb.d/db_backup.sql
