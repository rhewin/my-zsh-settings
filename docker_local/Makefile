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
	docker exec -it local-postgresql psql -U $$(grep POSTGRES_USER services/postgresql/.env | cut -d '=' -f2) -d $$(grep POSTGRES_DB services/postgresql/.env | cut -d '=' -f2)

rabbitmq-cli:
	docker exec -it local-rabbitmq rabbitmqctl status

mailpit-web:
	@echo "Opening Mailpit web interface at http://localhost:8025"
	@command -v xdg-open >/dev/null 2>&1 && xdg-open http://localhost:8025 || echo "Visit http://localhost:8025"

adminer-web:
	@echo "Opening Adminer web interface at http://localhost:8080"
	@command -v xdg-open >/dev/null 2>&1 && xdg-open http://localhost:8080 || echo "Visit http://localhost:8080"

rabbitmq-web:
	@echo "Opening RabbitMQ management interface at http://localhost:15672"
	@command -v xdg-open >/dev/null 2>&1 && xdg-open http://localhost:15672 || echo "Visit http://localhost:15672"

cleanup:
	docker-compose down -v --remove-orphans && docker system prune -f