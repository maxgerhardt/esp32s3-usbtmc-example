set(EXCLUDE_COMPONENTS esp_lcd) # otherwise we run into a compiler bug for that component
idf_component_register(
    SRCS
        main.c
        usb_descriptors.c
        usbtmc_app.c
        ../managed_components/espressif__tinyusb/hw/bsp/espressif/boards/family.c
    INCLUDE_DIRS
        .
        ../managed_components/espressif__tinyusb/hw
        ../managed_components/espressif__tinyusb/hw/bsp/espressif/boards/espressif_s3_devkitc
    REQUIRES tinyusb led_strip
)

# Add tusb_config.h directory to tinyusb component include path
idf_build_get_property(build_components BUILD_COMPONENTS)
if(tinyusb IN_LIST build_components)
    set(tinyusb_name tinyusb)
else()
    set(tinyusb_name espressif__tinyusb)
endif()
idf_component_get_property(tusb_lib ${tinyusb_name} COMPONENT_LIB)
target_include_directories(${tusb_lib} PRIVATE ".")