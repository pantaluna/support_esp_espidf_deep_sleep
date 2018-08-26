https://github.com/pantaluna/support_esp_espidf_deep_sleep

## Environment
- Development Kit:      [Adafruit HUZZAH32] I tried 2 boards
- Core (if using chip or module): [ESP-WROOM32]
- IDF version (commit id): ESP-IDF v3.2-dev-607-gb14e87a6 Aug25,2018  ESP-IDF
- Development Env:      [Eclipse]
- Operating System:     [Windows]
- Power Supply:         [USB from hub]

## Problem Description
The system does not wake up properly anymore after the ***2nd*** deep sleep period (and any deep sleep period after that) using ESP-IDF v3.2-dev-607-gb14e87a6. The app uses esp_sleep_enable_timer_wakeup().

@note The wakeup after the 1st deep sleep period always works fine. The esp_sleep_get_wakeup_cause() returns Timer and the value of an RTC_DATA_ATTR variable is kept alive.

The bootstrap messages are somewhat different at the beginning of the erronous wakeups. It seems to resets twice? :
```
ets Jun  8 2016 00:22:57

rst:0x5 (DEEPSLEEP_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
ets Jun  8 2016 00:22:57

rst:0x7 (TG0WDT_SYS_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0010,len:4
load:0x3fff0014,len:5940
load:0x40078000,len:9308
load:0x40080400,len:5780
entry 0x40080744
...
```

### Expected Behavior
The system should wakeup properly from all deep sleep periods.

### Actual Behavior

### Workaround
Go back to esp-idf tag v3.2-dev

### Steps to repropduce
No wiring needed.

```
cd ~
git clone --recursive https://github.com/pantaluna/support_esp_espidf_deep_sleep.git
cd  support_esp_espidf_deep_sleep
make erase_flash; make flash monitor
```

### Code to reproduce this issue
https://github.com/pantaluna/support_esp_espidf_deep_sleep

## Debug Logs
```
Python requirements are satisfied.
MONITOR
--- idf_monitor on COM12 115200 ---
--- Quit: Ctrl+] | Menu: Ctrl+T | Help: Ctrl+T followed by Ctrl+H ---
ets Jun  8 2016 00:22:57

rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0010,len:4
load:0x3fff0014,len:5940
load:0x40078000,len:9308
load:0x40080400,len:5780
entry 0x40080744
I (175) boot: ESP-IDF v3.2-dev-607-gb14e87a6 2nd stage bootloader
I (176) boot: compile time 23:01:09
I (176) boot: Enabling RNG early entropy source...
I (196) boot: SPI Speed      : 40MHz
I (209) boot: SPI Mode       : DIO
I (222) boot: SPI Flash Size : 4MB
I (235) boot: Partition Table:
I (246) boot: ## Label            Usage          Type ST Offset   Length
I (269) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (292) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (315) boot:  2 factory          factory app      00 00 00010000 00100000
I (339) boot: End of partition table
I (352) esp_image: segment 0: paddr=0x00010020 vaddr=0x3f400020 size=0x060b0 ( 24752) map
I (433) esp_image: segment 1: paddr=0x000160d8 vaddr=0x3ffb0000 size=0x0210c (  8460) load
I (456) esp_image: segment 2: paddr=0x000181ec vaddr=0x3ffb210c size=0x00000 (     0) load
I (457) esp_image: segment 3: paddr=0x000181f4 vaddr=0x40080000 size=0x00400 (  1024) load
0x40080000: _WindowOverflow4 at C:/myiot/esp/esp-idf/components/freertos/xtensa_vectors.S:1685

I (485) esp_image: segment 4: paddr=0x000185fc vaddr=0x40080400 size=0x07a14 ( 31252) load
I (590) esp_image: segment 5: paddr=0x00020018 vaddr=0x400d0018 size=0x11734 ( 71476) map
0x400d0018: _flash_cache_start at ??:?

I (745) esp_image: segment 6: paddr=0x00031754 vaddr=0x40087e14 size=0x01dbc (  7612) load
0x40087e14: rtc_clk_cpu_freq_get_config at C:/myiot/esp/esp-idf/components/soc/esp32/rtc_clk.c:472

I (767) esp_image: segment 7: paddr=0x00033518 vaddr=0x400c0000 size=0x00064 (   100) load
I (769) esp_image: segment 8: paddr=0x00033584 vaddr=0x50000000 size=0x00004 (     4) load
I (794) esp_image: segment 9: paddr=0x00033590 vaddr=0x50000004 size=0x00000 (     0) load
I (858) boot: Loaded app from partition at offset 0x10000
I (859) boot: Disabling RNG early entropy source...
I (864) cpu_start: Pro cpu up.
I (875) cpu_start: Starting app cpu, entry point is 0x40080f38
0x40080f38: call_start_cpu1 at C:/myiot/esp/esp-idf/components/esp32/cpu_start.c:226

I (1) cpu_start: App cpu up.
I (908) heap_init: Initializing. RAM available for dynamic allocation:
I (928) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (947) heap_init: At 3FFB3160 len 0002CEA0 (179 KiB): DRAM
I (966) heap_init: At 3FFE0440 len 00003BC0 (14 KiB): D/IRAM
I (986) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (1005) heap_init: At 40089BD0 len 00016430 (89 KiB): IRAM
I (1025) cpu_start: Pro cpu start user code
I (351) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (352) myapp: OK Task has been created, and is running right now
I (352) myapp: main_task()
I (352) myapp: END app_main()
I (352) myapp: *** Wakeup reason: UNKNOWN (not after a deep sleep period, probably after a normal reset)
I (372) myapp: ***(RTC_DATA_ATTR static uint32_t mcu_boot_count): 1

I (382) myapp:

***SECTION: DEEP SLEEP***
I (382) myapp: Entering deep sleep (the MCU should wake up 10 seconds later)...




ets Jun  8 2016 00:22:57

rst:0x5 (DEEPSLEEP_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0010,len:4
load:0x3fff0014,len:5940
load:0x40078000,len:9308
load:0x40080400,len:5780
entry 0x40080744
I (59) boot: ESP-IDF v3.2-dev-607-gb14e87a6 2nd stage bootloader
I (59) boot: compile time 23:01:09
I (60) boot: Enabling RNG early entropy source...
I (66) boot: SPI Speed      : 40MHz
I (70) boot: SPI Mode       : DIO
I (74) boot: SPI Flash Size : 4MB
I (79) boot: Partition Table:
I (82) boot: ## Label            Usage          Type ST Offset   Length
I (90) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (97) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (105) boot:  2 factory          factory app      00 00 00010000 00100000
I (113) boot: End of partition table
I (117) esp_image: segment 0: paddr=0x00010020 vaddr=0x3f400020 size=0x060b0 ( 24752) map
I (144) esp_image: segment 1: paddr=0x000160d8 vaddr=0x3ffb0000 size=0x0210c (  8460) load
I (152) esp_image: segment 2: paddr=0x000181ec vaddr=0x3ffb210c size=0x00000 (     0) load
I (152) esp_image: segment 3: paddr=0x000181f4 vaddr=0x40080000 size=0x00400 (  1024) load
0x40080000: _WindowOverflow4 at C:/myiot/esp/esp-idf/components/freertos/xtensa_vectors.S:1685

I (162) esp_image: segment 4: paddr=0x000185fc vaddr=0x40080400 size=0x07a14 ( 31252) load
I (197) esp_image: segment 5: paddr=0x00020018 vaddr=0x400d0018 size=0x11734 ( 71476) map
0x400d0018: _flash_cache_start at ??:?

I (248) esp_image: segment 6: paddr=0x00031754 vaddr=0x40087e14 size=0x01dbc (  7612) load
0x40087e14: rtc_clk_cpu_freq_get_config at C:/myiot/esp/esp-idf/components/soc/esp32/rtc_clk.c:472

I (256) esp_image: segment 7: paddr=0x00033518 vaddr=0x400c0000 size=0x00064 (   100)
I (256) esp_image: segment 8: paddr=0x00033584 vaddr=0x50000000 size=0x00004 (     4)
I (264) esp_image: segment 9: paddr=0x00033590 vaddr=0x50000004 size=0x00000 (     0)
I (285) boot: Loaded app from partition at offset 0x10000
I (285) boot: Disabling RNG early entropy source...
I (287) cpu_start: Pro cpu up.
I (291) cpu_start: Starting app cpu, entry point is 0x40080f38
0x40080f38: call_start_cpu1 at C:/myiot/esp/esp-idf/components/esp32/cpu_start.c:226

I (0) cpu_start: App cpu up.
I (302) heap_init: Initializing. RAM available for dynamic allocation:
I (308) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (315) heap_init: At 3FFB3160 len 0002CEA0 (179 KiB): DRAM
I (321) heap_init: At 3FFE0440 len 00003BC0 (14 KiB): D/IRAM
I (327) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (334) heap_init: At 40089BD0 len 00016430 (89 KiB): IRAM
I (340) cpu_start: Pro cpu start user code
I (350) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (351) myapp: OK Task has been created, and is running right now
I (351) myapp: main_task()
I (351) myapp: END app_main()
I (351) myapp: *** Wakeup reason: TIMER
I (361) myapp: ***(RTC_DATA_ATTR static uint32_t mcu_boot_count): 2

I (371) myapp:

***SECTION: DEEP SLEEP***
I (371) myapp: Entering deep sleep (the MCU should wake up 10 seconds later)...




ets Jun  8 2016 00:22:57

rst:0x5 (DEEPSLEEP_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
ets Jun  8 2016 00:22:57

rst:0x7 (TG0WDT_SYS_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0010,len:4
load:0x3fff0014,len:5940
load:0x40078000,len:9308
load:0x40080400,len:5780
entry 0x40080744
W (178) boot: PRO CPU has been reset by WDT.
W (179) boot: WDT reset info: PRO CPU PC=0xdfa8224
W (179) boot: WDT reset info: APP CPU PC=0xbc77af4a
I (199) boot: ESP-IDF v3.2-dev-607-gb14e87a6 2nd stage bootloader
I (220) boot: compile time 23:01:09
I (233) boot: Enabling RNG early entropy source...
I (250) boot: SPI Speed      : 40MHz
I (263) boot: SPI Mode       : DIO
I (275) boot: SPI Flash Size : 4MB
I (288) boot: Partition Table:
I (299) boot: ## Label            Usage          Type ST Offset   Length
I (322) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (345) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (369) boot:  2 factory          factory app      00 00 00010000 00100000
I (392) boot: End of partition table
I (405) esp_image: segment 0: paddr=0x00010020 vaddr=0x3f400020 size=0x060b0 ( 24752) map
I (487) esp_image: segment 1: paddr=0x000160d8 vaddr=0x3ffb0000 size=0x0210c (  8460) load
I (509) esp_image: segment 2: paddr=0x000181ec vaddr=0x3ffb210c size=0x00000 (     0) load
I (511) esp_image: segment 3: paddr=0x000181f4 vaddr=0x40080000 size=0x00400 (  1024) load
0x40080000: _WindowOverflow4 at C:/myiot/esp/esp-idf/components/freertos/xtensa_vectors.S:1685

I (538) esp_image: segment 4: paddr=0x000185fc vaddr=0x40080400 size=0x07a14 ( 31252) load
I (643) esp_image: segment 5: paddr=0x00020018 vaddr=0x400d0018 size=0x11734 ( 71476) map
0x400d0018: _flash_cache_start at ??:?

I (799) esp_image: segment 6: paddr=0x00031754 vaddr=0x40087e14 size=0x01dbc (  7612) load
0x40087e14: rtc_clk_cpu_freq_get_config at C:/myiot/esp/esp-idf/components/soc/esp32/rtc_clk.c:472

I (820) esp_image: segment 7: paddr=0x00033518 vaddr=0x400c0000 size=0x00064 (   100) load
I (822) esp_image: segment 8: paddr=0x00033584 vaddr=0x50000000 size=0x00004 (     4) load
I (847) esp_image: segment 9: paddr=0x00033590 vaddr=0x50000004 size=0x00000 (     0) load
I (911) boot: Loaded app from partition at offset 0x10000
I (912) boot: Disabling RNG early entropy source...
I (918) cpu_start: Pro cpu up.
I (928) cpu_start: Starting app cpu, entry point is 0x40080f38
0x40080f38: call_start_cpu1 at C:/myiot/esp/esp-idf/components/esp32/cpu_start.c:226

I (1) cpu_start: App cpu up.
I (961) heap_init: Initializing. RAM available for dynamic allocation:
I (982) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (1000) heap_init: At 3FFB3160 len 0002CEA0 (179 KiB): DRAM
I (1020) heap_init: At 3FFE0440 len 00003BC0 (14 KiB): D/IRAM
I (1039) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (1059) heap_init: At 40089BD0 len 00016430 (89 KiB): IRAM
I (1079) cpu_start: Pro cpu start user code
I (368) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (370) myapp: OK Task has been created, and is running right now
I (370) myapp: main_task()
I (370) myapp: END app_main()
I (370) myapp: *** Wakeup reason: UNKNOWN (not after a deep sleep period, probably after a normal reset)
I (390) myapp: ***(RTC_DATA_ATTR static uint32_t mcu_boot_count): 1

I (400) myapp:

***SECTION: DEEP SLEEP***
I (400) myapp: Entering deep sleep (the MCU should wake up 10 seconds later)...




ets Jun  8 2016 00:22:57

rst:0x5 (DEEPSLEEP_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
ets Jun  8 2016 00:22:57

rst:0x7 (TG0WDT_SYS_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0010,len:4
load:0x3fff0014,len:5940
load:0x40078000,len:9308
load:0x40080400,len:5780
entry 0x40080744
W (178) boot: PRO CPU has been reset by WDT.
W (179) boot: WDT reset info: PRO CPU PC=0xdfa8224
W (179) boot: WDT reset info: APP CPU PC=0xbc77af0a
I (199) boot: ESP-IDF v3.2-dev-607-gb14e87a6 2nd stage bootloader
I (220) boot: compile time 23:01:09
I (233) boot: Enabling RNG early entropy source...
I (250) boot: SPI Speed      : 40MHz
I (263) boot: SPI Mode       : DIO
I (275) boot: SPI Flash Size : 4MB
I (288) boot: Partition Table:
I (299) boot: ## Label            Usage          Type ST Offset   Length
I (322) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (345) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (369) boot:  2 factory          factory app      00 00 00010000 00100000
I (392) boot: End of partition table
I (405) esp_image: segment 0: paddr=0x00010020 vaddr=0x3f400020 size=0x060b0 ( 24752) map
I (487) esp_image: segment 1: paddr=0x000160d8 vaddr=0x3ffb0000 size=0x0210c (  8460) load
I (509) esp_image: segment 2: paddr=0x000181ec vaddr=0x3ffb210c size=0x00000 (     0) load
I (511) esp_image: segment 3: paddr=0x000181f4 vaddr=0x40080000 size=0x00400 (  1024) load
0x40080000: _WindowOverflow4 at C:/myiot/esp/esp-idf/components/freertos/xtensa_vectors.S:1685

I (538) esp_image: segment 4: paddr=0x000185fc vaddr=0x40080400 size=0x07a14 ( 31252) load
I (643) esp_image: segment 5: paddr=0x00020018 vaddr=0x400d0018 size=0x11734 ( 71476) map
0x400d0018: _flash_cache_start at ??:?

I (799) esp_image: segment 6: paddr=0x00031754 vaddr=0x40087e14 size=0x01dbc (  7612) load
0x40087e14: rtc_clk_cpu_freq_get_config at C:/myiot/esp/esp-idf/components/soc/esp32/rtc_clk.c:472

I (820) esp_image: segment 7: paddr=0x00033518 vaddr=0x400c0000 size=0x00064 (   100) load
I (822) esp_image: segment 8: paddr=0x00033584 vaddr=0x50000000 size=0x00004 (     4) load
I (847) esp_image: segment 9: paddr=0x00033590 vaddr=0x50000004 size=0x00000 (     0) load
I (911) boot: Loaded app from partition at offset 0x10000
I (912) boot: Disabling RNG early entropy source...
I (918) cpu_start: Pro cpu up.
I (928) cpu_start: Starting app cpu, entry point is 0x40080f38
0x40080f38: call_start_cpu1 at C:/myiot/esp/esp-idf/components/esp32/cpu_start.c:226

I (1) cpu_start: App cpu up.
I (961) heap_init: Initializing. RAM available for dynamic allocation:
I (982) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (1000) heap_init: At 3FFB3160 len 0002CEA0 (179 KiB): DRAM
I (1020) heap_init: At 3FFE0440 len 00003BC0 (14 KiB): D/IRAM
I (1039) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (1059) heap_init: At 40089BD0 len 00016430 (89 KiB): IRAM
I (1079) cpu_start: Pro cpu start user code
I (368) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (370) myapp: OK Task has been created, and is running right now
I (370) myapp: main_task()
I (370) myapp: END app_main()
I (370) myapp: *** Wakeup reason: UNKNOWN (not after a deep sleep period, probably after a normal reset)
I (390) myapp: ***(RTC_DATA_ATTR static uint32_t mcu_boot_count): 1

I (400) myapp:

***SECTION: DEEP SLEEP***
I (400) myapp: Entering deep sleep (the MCU should wake up 10 seconds later)...




ets Jun  8 2016 00:22:57

rst:0x5 (DEEPSLEEP_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
ets Jun  8 2016 00:22:57

rst:0x7 (TG0WDT_SYS_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
...

```


##################################################################################################
# APPENDICES

# 1. SOP for upload to GITHUB
https://github.com/pantaluna/support_esp_espidf_deep_sleep

## 1.a: BROWSER: create github public repo support_esp_espidf_deep_sleep of pantaluna at Github.com

## 1.b: MSYS2 git
```
git config --system --unset credential.helper
git config credential.helper store
git config --list

cd  /c/myiot/esp/support_esp_espidf_deep_sleep
git init
git add .
git commit -m "ESP-IDF support ticket. First commit"
git remote add origin https://github.com/pantaluna/support_esp_espidf_deep_sleep.git

git push --set-upstream origin master

git remote show origin
git status

git tag --annotate v1.0 --message "First release"
git push origin --tags

```

# 2. SOP for source updates
```
cd  /c/myiot/esp/support_esp_espidf_deep_sleep
git add .
git commit -m "Updated after testing xxx"

git push --set-upstream origin master

git status
```
