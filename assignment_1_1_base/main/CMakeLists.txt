idf_component_register(SRCS "app_main.c" "alarm.c"
REQUIRES nvs_flash esp_event esp_netif esp_timer mqtt temperature_sensor
                    INCLUDE_DIRS ".")
