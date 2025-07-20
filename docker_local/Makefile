up:
	docker-compose up -d

down:
	docker-compose down

restart:
	docker-compose down && docker-compose up -d

logs:
	docker-compose logs -f

mysql-cli:
	docker exec -it local-mysql mysql -uroot -p$$(grep MYSQL_ROOT_PASSWORD services/mysql/.env | cut -d '=' -f2)

psql-cli:
	docker exec -it local-postgres psql -U $$(grep POSTGRES_USER services/postgres/.env | cut -d '=' -f2) -d $$(grep POSTGRES_DB services/postgres/.env | cut -d '=' -f2)

cleanup:
	docker-compose down -v --remove-orphans && docker system prune -f