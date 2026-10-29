# 新建工程

nRF5 SDK工程模板：

+ 裸机模板：`examples/peripheral/template_project`
+ 蓝牙模板：`examples/ble_peripheral/ble_app_template`

## SDK开发流程简介

### 为什么需要SDK？

我们最先熟悉的是通用MCU开发：如stm32。经典开发方式：从零开始新建工程添加驱动库，然后驱动一个个硬件外设。

但蓝牙或WiFi的开发方式则完全不同。这些无线SoC相当复杂，我们几乎没可能从零开始进行开发，而是基于厂商提供的SDK和demo例程来参考学习。

### MCU开发与蓝牙SDK开发的差异

+ 通用MCU：外设标准化彼此独立。外设库只是薄封装。**开发核心是配置硬件**
+ 蓝牙芯片：蓝牙是射频、链路层、协议栈的完整通信系统，SDK承载了这套系统不可绕过。**开发核心是在协议栈上做应用层profile和业务逻辑**

MCU开发与蓝牙SDK开发的差异：

1. 外设库/SDK角色不同

    + MCU外设库：效率工具，可直接操作寄存器替代
    + 蓝牙SDK：协议栈载体和认证基线，绕开代价极高

2. 复杂度来源不同

    + MCU：外设多但独立、行为确定
    + 蓝牙：协议分层深、状态机复杂、操作性要求高

3. 开发重点不同

    + MCU：底层外设配置
    + 蓝牙：应用层服务/profile设计与业务

一句话总结：

MCU外设：可以自己理解并直接控制的硬件，外设库配置自由
蓝牙SDK：只能信任并使用的基础通信设施，SDK服务于操作可信

## nRF5 SDK工程模板

前面介绍了蓝牙的SDK开发。我们不要想着自己去新建工程了，直接用SDK提供的工程模板开发。nRF5 SDK提供了2个工程模板：

+ 裸机模板：`examples/peripheral/template_project`
+ 蓝牙模板：`examples/ble_peripheral/ble_app_template`

### `template_project`分析

```xml
<!DOCTYPE CrossStudio_Project_File>
<solution Name="template_pca10056" target="8" version="2">
  <project Name="template_pca10056">
    <configuration
      Name="Common"
      arm_architecture="v7EM"
      arm_core_type="Cortex-M4"
      arm_endian="Little"
      arm_fp_abi="Hard"
      arm_fpu_type="FPv4-SP-D16"
      arm_linker_heap_size="8192"
      arm_linker_process_stack_size="0"
      arm_linker_stack_size="8192"
      arm_linker_treat_warnings_as_errors="No"
      arm_simulator_memory_simulation_parameter="RWX 00000000,00100000,FFFFFFFF;RWX 20000000,00010000,CDCDCDCD"
      arm_target_device_name="nRF52840_xxAA"
      arm_target_interface_type="SWD"
      c_user_include_directories="../../../config;../../../../../../components;../../../../../../components/boards;../../../../../../components/drivers_nrf/nrf_soc_nosd;../../../../../../components/libraries/atomic;../../../../../../components/libraries/atomic_fifo;../../../../../../components/libraries/balloc;../../../../../../components/libraries/block_dev;../../../../../../components/libraries/bsp;../../../../../../components/libraries/button;../../../../../../components/libraries/cli;../../../../../../components/libraries/cli/uart;../../../../../../components/libraries/crc16;../../../../../../components/libraries/crc32;../../../../../../components/libraries/crypto;../../../../../../components/libraries/crypto/backend/cc310;../../../../../../components/libraries/crypto/backend/cc310_bl;../../../../../../components/libraries/crypto/backend/cifra;../../../../../../components/libraries/crypto/backend/mbedtls;../../../../../../components/libraries/crypto/backend/micro_ecc;../../../../../../components/libraries/crypto/backend/nrf_hw;../../../../../../components/libraries/crypto/backend/nrf_sw;../../../../../../components/libraries/crypto/backend/oberon;../../../../../../components/libraries/crypto/backend/optiga;../../../../../../components/libraries/csense;../../../../../../components/libraries/csense_drv;../../../../../../components/libraries/delay;../../../../../../components/libraries/ecc;../../../../../../components/libraries/experimental_section_vars;../../../../../../components/libraries/experimental_task_manager;../../../../../../components/libraries/fds;../../../../../../components/libraries/fifo;../../../../../../components/libraries/fstorage;../../../../../../components/libraries/gfx;../../../../../../components/libraries/gpiote;../../../../../../components/libraries/hardfault;../../../../../../components/libraries/hardfault/nrf52;../../../../../../components/libraries/hci;../../../../../../components/libraries/led_softblink;../../../../../../components/libraries/log;../../../../../../components/libraries/log/src;../../../../../../components/libraries/low_power_pwm;../../../../../../components/libraries/mem_manager;../../../../../../components/libraries/memobj;../../../../../../components/libraries/mpu;../../../../../../components/libraries/mutex;../../../../../../components/libraries/pwm;../../../../../../components/libraries/pwr_mgmt;../../../../../../components/libraries/queue;../../../../../../components/libraries/ringbuf;../../../../../../components/libraries/scheduler;../../../../../../components/libraries/sdcard;../../../../../../components/libraries/slip;../../../../../../components/libraries/sortlist;../../../../../../components/libraries/spi_mngr;../../../../../../components/libraries/stack_guard;../../../../../../components/libraries/stack_info;../../../../../../components/libraries/strerror;../../../../../../components/libraries/timer;../../../../../../components/libraries/twi_mngr;../../../../../../components/libraries/twi_sensor;../../../../../../components/libraries/uart;../../../../../../components/libraries/usbd;../../../../../../components/libraries/usbd/class/audio;../../../../../../components/libraries/usbd/class/cdc;../../../../../../components/libraries/usbd/class/cdc/acm;../../../../../../components/libraries/usbd/class/hid;../../../../../../components/libraries/usbd/class/hid/generic;../../../../../../components/libraries/usbd/class/hid/kbd;../../../../../../components/libraries/usbd/class/hid/mouse;../../../../../../components/libraries/usbd/class/msc;../../../../../../components/libraries/util;../../../../../../components/toolchain/cmsis/include;../../..;../../../../../../external/cifra_AES128-EAX;../../../../../../external/fnmatch;../../../../../../external/fprintf;../../../../../../external/mbedtls/include;../../../../../../external/micro-ecc/micro-ecc;../../../../../../external/nrf_cc310/include;../../../../../../external/nrf_oberon;../../../../../../external/nrf_oberon/include;../../../../../../external/nrf_tls/mbedtls/nrf_crypto/config;../../../../../../external/protothreads;../../../../../../external/protothreads/pt-1.4;../../../../../../external/thedotfactory_fonts;../../../../../../external/utf_converter;../../../../../../integration/nrfx;../../../../../../integration/nrfx/legacy;../../../../../../modules/nrfx;../../../../../../modules/nrfx/drivers/include;../../../../../../modules/nrfx/hal;../../../../../../modules/nrfx/mdk;../config;"
      c_preprocessor_definitions="APP_TIMER_V2;APP_TIMER_V2_RTC1_ENABLED;BOARD_PCA10056;CONFIG_GPIO_AS_PINRESET;FLOAT_ABI_HARD;INITIALIZE_USER_SECTIONS;MBEDTLS_CONFIG_FILE=&quot;nrf_crypto_mbedtls_config.h&quot;;NO_VTOR_CONFIG;NRF52840_XXAA;NRF_CRYPTO_MAX_INSTANCE_COUNT=1;uECC_ENABLE_VLI_API=0;uECC_OPTIMIZATION_LEVEL=3;uECC_SQUARE_FUNC=0;uECC_SUPPORT_COMPRESSED_POINT=0;uECC_VLI_NATIVE_LITTLE_ENDIAN=1;"
      debug_target_connection="J-Link"
      gcc_entry_point="Reset_Handler"
      macros="CMSIS_CONFIG_TOOL=../../../../../../external_tools/cmsisconfig/CMSIS_Configuration_Wizard.jar"
      debug_register_definition_file="../../../../../../modules/nrfx/mdk/nrf52840.svd"
      debug_start_from_entry_point_symbol="No"
      gcc_debugging_level="Level 3"      linker_output_format="hex"
      linker_printf_width_precision_supported="Yes"
      linker_printf_fmt_level="long"
      linker_scanf_fmt_level="long"
      linker_section_placement_file="flash_placement.xml"
      linker_section_placement_macros="FLASH_PH_START=0x0;FLASH_PH_SIZE=0x100000;RAM_PH_START=0x20000000;RAM_PH_SIZE=0x40000;FLASH_START=0x0;FLASH_SIZE=0x100000;RAM_START=0x20000000;RAM_SIZE=0x40000"
      
      linker_section_placements_segments="FLASH1 RX 0x0 0x100000;RAM1 RWX 0x20000000 0x40000"
      project_directory=""
      project_type="Executable" />
      <folder Name="Segger Startup Files">
        <file file_name="$(StudioDir)/source/thumb_crt0.s" />
      </folder>
    <folder Name="nrf_cc310">
      <file file_name="../../../../../../external/nrf_cc310/lib/cortex-m4/hard-float/libnrf_cc310_0.9.13.a" />
    </folder>
    <folder Name="Board Definition">
      <file file_name="../../../../../../components/boards/boards.c" />
    </folder>
    <folder Name="nRF_Crypto backend uECC">
      <file file_name="../../../../../../components/libraries/crypto/backend/micro_ecc/micro_ecc_backend_ecc.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/micro_ecc/micro_ecc_backend_ecdh.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/micro_ecc/micro_ecc_backend_ecdsa.c" />
    </folder>
    <folder Name="nRF_Drivers">
      <file file_name="../../../../../../integration/nrfx/legacy/nrf_drv_clock.c" />
      <file file_name="../../../../../../integration/nrfx/legacy/nrf_drv_power.c" />
      <file file_name="../../../../../../integration/nrfx/legacy/nrf_drv_ppi.c" />
      <file file_name="../../../../../../integration/nrfx/legacy/nrf_drv_rng.c" />
      <file file_name="../../../../../../integration/nrfx/legacy/nrf_drv_spi.c" />
      <file file_name="../../../../../../integration/nrfx/legacy/nrf_drv_spis.c" />
      省略...
      <file file_name="../../../../../../modules/nrfx/drivers/src/nrfx_uart.c" />
      <file file_name="../../../../../../modules/nrfx/drivers/src/nrfx_uarte.c" />
      <file file_name="../../../../../../modules/nrfx/drivers/src/nrfx_usbd.c" />
      <file file_name="../../../../../../modules/nrfx/drivers/src/nrfx_wdt.c" />
    </folder>
    <folder Name="nRF_Crypto backend cifra">
      <file file_name="../../../../../../components/libraries/crypto/backend/cifra/cifra_backend_aes_aead.c" />
    </folder>
    <folder Name="nRF_External">
      <file file_name="../../../../../../external/thedotfactory_fonts/orkney24pts.c" />
      <file file_name="../../../../../../external/thedotfactory_fonts/orkney8pts.c" />
    </folder>
    <folder Name="Application">
      <file file_name="../../../main.c" />
      <file file_name="../config/sdk_config.h" />
    </folder>
    <folder Name="Board Support">
      <file file_name="../../../../../../components/libraries/bsp/bsp.c" />
    </folder>
    <folder Name="nRF_Crypto backend nRF HW">
      <file file_name="../../../../../../components/libraries/crypto/backend/nrf_hw/nrf_hw_backend_init.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/nrf_hw/nrf_hw_backend_rng.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/nrf_hw/nrf_hw_backend_rng_mbedtls.c" />
    </folder>
    <folder Name="nRF_Drivers_External">
      <file file_name="../../../../../../components/libraries/twi_sensor/nrf_twi_sensor.c" />
    </folder>
    <folder Name="nRF_micro-ecc">
      <file file_name="../../../../../../external/micro-ecc/nrf52hf_armgcc/armgcc/micro_ecc_lib_nrf52.a" />
    </folder>
    <folder Name="nRF_Crypto backend CC310">
      <file file_name="../../../../../../components/libraries/crypto/backend/cc310/cc310_backend_aes.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/cc310/cc310_backend_aes_aead.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/cc310/cc310_backend_chacha_poly_aead.c" />
      省略...
      <file file_name="../../../../../../components/libraries/crypto/backend/cc310/cc310_backend_rng.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/cc310/cc310_backend_shared.c" />
    </folder>
    <folder Name="nRF_Crypto backend Oberon">
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_chacha_poly_aead.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_ecc.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_ecdh.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_ecdsa.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_eddsa.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_hash.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/oberon/oberon_backend_hmac.c" />
    </folder>
    <folder Name="nRF_Crypto backend mbed TLS">
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_aes.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_aes_aead.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_ecc.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_ecdh.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_ecdsa.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_hash.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_hmac.c" />
      <file file_name="../../../../../../components/libraries/crypto/backend/mbedtls/mbedtls_backend_init.c" />
    </folder>
    <folder Name="nRF_Oberon_Crypto">
      <file file_name="../../../../../../external/nrf_oberon/lib/cortex-m4/hard-float/liboberon_3.0.8.a" />
    </folder>
    <folder Name="nRF_TLS">
      <file file_name="../../../../../../external/mbedtls/library/aes.c" />
      <file file_name="../../../../../../external/mbedtls/library/aesni.c" />
      <file file_name="../../../../../../external/mbedtls/library/arc4.c" />
      <file file_name="../../../../../../external/mbedtls/library/aria.c" />
      <省略...
      <file file_name="../../../../../../external/mbedtls/library/x509_crt.c" />
      <file file_name="../../../../../../external/mbedtls/library/x509_csr.c" />
      <file file_name="../../../../../../external/mbedtls/library/x509write_crt.c" />
      <file file_name="../../../../../../external/mbedtls/library/x509write_csr.c" />
      <file file_name="../../../../../../external/mbedtls/library/xtea.c" />
    </folder>
    <folder Name="nRF_Log">
      <file file_name="../../../../../../components/libraries/log/src/nrf_log_frontend.c" />
      <file file_name="../../../../../../components/libraries/log/src/nrf_log_str_formatter.c" />
    </folder>
    <folder Name="nRF_Core">
      <file file_name="../../../../../../components/libraries/mpu/nrf_mpu_lib.c" />
      <file file_name="../../../../../../components/libraries/stack_guard/nrf_stack_guard.c" />
    </folder>
    <folder Name="nRF_Libraries">
      <file file_name="../../../../../../components/libraries/button/app_button.c" />
      <file file_name="../../../../../../components/libraries/util/app_error.c" />
      <file file_name="../../../../../../components/libraries/util/app_error_handler_gcc.c" />
      <file file_name="../../../../../../components/libraries/util/app_error_weak.c" />
      省略...
      <file file_name="../../../../../../components/libraries/slip/slip.c" />
      <file file_name="../../../../../../components/libraries/experimental_task_manager/task_manager.c" />
      <file file_name="../../../../../../components/libraries/experimental_task_manager/task_manager_core_armgcc.S" />
    </folder>
    <folder Name="nRF_Crypto">
      <file file_name="../../../../../../components/libraries/crypto/nrf_crypto_aead.c" />
      <file file_name="../../../../../../components/libraries/crypto/nrf_crypto_aes.c" />
      省略...
      <file file_name="../../../../../../components/libraries/crypto/nrf_crypto_hmac.c" />
      <file file_name="../../../../../../components/libraries/crypto/nrf_crypto_init.c" />
      <file file_name="../../../../../../components/libraries/crypto/nrf_crypto_rng.c" />
      <file file_name="../../../../../../components/libraries/crypto/nrf_crypto_shared.c" />
    </folder>
    <folder Name="None">
      <file file_name="../../../../../../modules/nrfx/mdk/ses_startup_nrf52840.s" />
      <file file_name="../../../../../../modules/nrfx/mdk/ses_startup_nrf_common.s" />
      <file file_name="../../../../../../modules/nrfx/mdk/system_nrf52840.c" />
    </folder>
    <folder Name="UTF8/UTF16 converter">
      <file file_name="../../../../../../external/utf_converter/utf.c" />
    </folder>
  </project>
  <configuration Name="Release"
    c_preprocessor_definitions="NDEBUG"
    link_time_optimization="No"    gcc_optimization_level="Optimize For Size" />
  <configuration Name="Debug"
    c_preprocessor_definitions="DEBUG; DEBUG_NRF"
    gcc_optimization_level="None"/>

</solution>
```

+ Arm 为 8，RISC-V 为 20