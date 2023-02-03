# Keyboard Config

I've made a number of changes to the firmware of my [Keychron K6 Pro](https://www.keychron.com/pages/keychron-k6-pro-user-guide), but don't want to be locked down to one specific fork of [qmk_firmware](https://github.com/qmk/qmk_firmware). 

This board has bluetooth capability, which is only included in the [bluetooth_playground](https://github.com/Keychron/qmk_firmware/tree/bluetooth_playground) branch of Keychron's fork of [qmk_firmware](https://github.com/Keychron/qmk_firmware). 

I prefer Vial to Via because, unlike Via, Vial is open-source and stores a copy of the keymap on the keyboard.

# Changes

1. Make the battery level animation quicker in `keyboards/keychron/bluetooth/bat_level_animation.c`
    ```
    #ifndef BAT_LEVEL_GROWING_INTERVAL
    #    define BAT_LEVEL_GROWING_INTERVAL 80 // ORIGINALLY 150
    #endif

    #ifndef BAT_LEVEL_ON_INTERVAL
    #    define BAT_LEVEL_ON_INTERVAL 1000 // ORIGINALLY 3000
    #endif
    ```
1. Add code that changes which layers the dip switch toggles between in `keyboards/keychron/k6_pro/k6_pro.c`
    ```
    bool dip_switch_update_kb(uint8_t index, bool active) {
        if (index == 0) {
            // -------------------THIS IS THE DEFAULT SETTING-------------------
            // Mac/iOS side of switch is layer 0 and Win/Android side of switch is layer 1
            // default_layer_set(1UL << (active ? 1 : 0));

            // -------------------THIS IS MY CUSTOM SETTING-------------------
            // Mac/iOS side of switch is layer 0 and Win/Android side of switch is layer 3
            default_layer_set(1UL << (active ? 3 : 0));
        }
        dip_switch_update_user(index, active);

        return true;
    }
    ```
1. Give the customKeycodes better names and titles in the `keyboards/keychron/k6_pro/ansi/rgb/keymaps/vial/vial.json`
    ```
    "customKeycodes": [
        {"name": "Left Opt", "title": "Left Option", "shortName": "LOpt"},
        {"name": "Right Opt", "title": "Right Option", "shortName": "ROpt"},
        {"name": "Left Cmd", "title": "Left Command", "shortName": "LCmd"},
        {"name": "Right Cmd", "title": "Right Command", "shortName": "RCmd"},
        {"name": "Misson Control", "title": "Opens Misson Control, availabe in macOS", "shortName": "MCtrl"},
        {"name": "Launch Pad", "title": "Opens Launchpad, availabe in macOS", "shortName": "LPad"},
        {"name": "Task View", "title": "Opens Task View in Windows", "shortName": "Task"},
        {"name": "File Explorer", "title": "Opens File Explorer in Windows", "shortName": "FileExp"},
        {"name": "Screen Shot", "title": "Screenshot in macOS", "shortName": "ScrnSt"},
        {"name": "Cortana", "title": "Opens Cortana in Windows", "shortName": "Cortana"},
        {"name": "Siri", "title": "Opens Siri in macOS", "shortName": "Siri"},
        {"name": "Bluetooth Host 1", "title": "Bluetooth Host 1", "shortName": "BTH1"},
        {"name": "Bluetooth Host 2", "title": "Bluetooth Host 2", "shortName": "BTH2"},
        {"name": "Bluetooth Host 3", "title": "Bluetooth Host 3", "shortName": "BTH3"},
        {"name": "Battery Level", "title": "Show battery level", "shortName": "Batt"}
    ],
    ```
1. Rearrange layers and reassign keys in the `keyboards/keychron/k6_pro/ansi/rgb/keymaps/vial/keymap.c` (View a copy of my changes in [keymap.c](keymap.c))
