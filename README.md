Logger test library (WIP)
========================

## Overview
 
 * Docker (nginx, php-fpm, mysql, phpmyadmin)
 * Symfony 3.4
 * Monolog configuration
 * Controller (to test logs)

 ## Install
 
 Create a ` .env ` file with the configuration in ` .env.dist `
 
 ` docker-compose build `
 
 ` docker-compose up -d `
 
 ` docker-compose start `
 
 #### Container
 
 ` docker-compose exec php bash `
 
 ## Route to trigger logs
 
 ` localhost:8091/logs `
 
 ## Access to ELK (Graylog)
 
 ` localhost:9000 `
 
 ## Access to Sentry
  
  Depends on your ` DSN key ` inside ` parameters.yml `
 
 ## Configuration
 
 ```yml
 monolog:
     channels:
         - my_channel
     handlers:
         main:
             type: stream
             path: '%kernel.logs_dir%/%kernel.environment%.log'
             level: notice
             channels: ['!event']
         gelf:
             type: gelf
             publisher:
                 hostname: '%gelf_host%'
                 port: '%gelf_port%'
             level: info
         sentry:
             type: raven
             dsn: '%sentry_dsn%'
             level: warning
         finger:
             type: fingers_crossed
             action_level: error
             handler: sentry_error
         sentry_error:
             type: raven
             dsn: '%sentry_dsn%'
             level: info
         gelf_channel_specific:
             type: gelf
             publisher:
                 hostname: '%gelf_host%'
                 port: '%gelf_port%'
             level: info
             channels: ['my_channel']
 ``` 