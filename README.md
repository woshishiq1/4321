
set -e
#----------------
    # Working instructions 
    # Language settings available:
    # English `en`
    # Russian `ru` 
    # Indonesian `id`
    # Chinese `zh`
    # Hindi `hi`
    
LANGUAGE=en

#----------------
    # The `FORCE_START` option forces the script to start without selection from the menu using volume buttons. The script will output an error if the configuration is not set up correctly. 
    # There should be no values of `ask` in any of the arguments. 
    # Available values `false` or `true`.

FORCE_START=false

#----------------
    # Configure the option if you receive error 36.1 fstab not found. This can happen if the ro.hardware variable differs in TWRP and the running system
    # You can check this in TWRP terminal by typing the command `getprop ro.hardware` and in the running system through any terminal, if the props differ, you need to enter the value that is displayed in the running system
    # Otherwise, leave the value blank

FSTAB_EXTENSION=auto

#----------------
    # The option disables system integrity check
    # Available options false, true, ask

DISABLE_VERITY_VBMETA_PATCH=ask

#----------------
    # The option allows hiding the absence of encryption /data, only works if selinux is not set to enforcing mode, also works if Magisk or KernelSU is installed
    # Available options false, true, ask

HIDE_NOT_ENCRYPTED=ask

#----------------
    # *** The option will work only if Magisk/KernelSu is installed or if you have slinux=premisive
    # Custom props, will be set at the stage at which you specified
    # Usage example: 
    # `CUSTOM_SETPROP="--init my.prop=value my.prop2=value my.prop3=value --early-fs my.prop=value my.prop2=value my.prop3=value"` and so on
    # Available stages init: `--init`, `--early-fs`, `--post-fs-data`, `--boot_completed`
    # Otherwise, leave it empty

CUSTOM_SETPROP=""

#----------------
    # The option to Add Custom Denylist:
    # This option writes application packages to `denylist` at boot time. Works only if `Magisk` is installed.
    # You can manually configure the configuration file in `.zip/META-INF/tools/denylist.txt`.
    # Available options:  
    # `false` - disable, 
    # `ask` - prompt during installation,
    # `first_time_boot` - the script will be executed only once during the first boot, the record of the first use is stored in Magisk memory,
    # `always_on_boot` - the script will be executed at every system boot.

INJECT_CUSTOM_DENYLIST_IN_BOOT=ask

#----------------
    # Enable `Zygisk` Mode:
    # This option forcibly enables `zygisk` mode on device startup, even if `Magisk` is installed for the first time.
    # Available options: 
    # `false` - disable, `ask` - prompt during installation,
    # `first_time_boot` - the script will be executed only once during the first boot, the record of the first use is stored in Magisk memory,
    # `always_on_boot` - the script will be executed at every system boot.

ZYGISK_TURN_ON_IN_BOOT=ask

#----------------
    # Enables built-in security fix integrated into dfe-neo, which runs at the initialization stage during boot, works only if selinux is not set to enforcing mode, also works if Magisk or KernelSU is installed
    # Available options false, true, ask

SAFETY_NET_FIX_PATCH=ask


#----------------
    # Set to `true` to remove PIN lock, `false` otherwise
    # Set to `ask` to prompt the user during installation

REMOVE_LOCKSCREEN_INFO=ask

#----------------
    # Set to `true` to wipe data during installation, `false` otherwise
    # Set to `ask` to prompt the user during installation

WIPE_DATA_AFTER_INSTALL=false

#----------------
    # This option specifies whether fstab substitution should also be done in --early as well as in --late init when mounting partitions from fstab. --early mount includes all partitions except those with first_stage_mount and latemount flags set.
    # By default, it is set to false.
    # If set to `ask`, the script will prompt the user to choose the option during installation.

MOUNT_FSTAB_EARLY_TOO=ask

#----------------
    # Block for setting to remove or replace patterns in fstab. Leave default if you don't know what you need it for.
    #   `-m` specifies the mount point line where patterns should be removed. For example, `-m /data`. After this flag, specify `-r and/or -p`.
    #   `-r` specifies which patterns to remove. Patterns will be removed up to comma or space. For example:
    #        /.../userdata	/data	f2fs	noatime,....,inlinecrypt	wait,....,fileencryption=aes-256-xts:aes-256-cts:v2,....,fscompress
    #        with `-m /data -r fileencryption= inlinecrypt` fileencryption=aes-256-xts:aes-256-cts:v2 will be removed. Resulting line will be:
    #        /.../userdata	/data	f2fs	noatime,....	wait,....,....,fscompress
    #   `-p` specifies which patterns to replace. For example, `-m /data -p inlinecrypt--to--ecrypt`. Result will be:
    #        /.../userdata	/data	f2fs	noatime,....,ecrypt	wait,....,fileencryption=aes-256-xts:aes-256-cts:v2,....,fscompress
    #        You can specify multiple parameters `-p inlinecrypt--to--ecrypt fileencryption--to--notencryption`
    #   `-v` When this flag is specified, all lines in fstab starting with `overlay` will be commented out, thereby disabling the manufacturer's system overlay. To feel the effect, set true for the modify_early_mount option.
    #   Example of filling:
    #        "-m /data -p fileencryption--to--notencrypteble ice--to--not-ice -r forceencrypt= -m /system -p ro--to--rw -m /metadata -r keydirectory="
    #        Default value: "-m /data -r fileencryption= forcefdeorfbe= encryptable= forceencrypt= metadata_encryption= keydirectory= inlinecrypt quota wrappedkey"

FSTAB_PATCH_PATERNS="-m /data -r fileencryption= forcefdeorfbe= encryptable= forceencrypt= metadata_encryption= keydirectory= inlinecrypt quota wrappedkey"

#----------------
    # Configuration Section: Injection Options
    #  `WHERE_TO_INJECT`: This option determines where the module will be injected. Choose one of the options:
    #      `super`: The module will be flashed into the current slot next to the system, vendor, etc.
    #      `vendor_boot`: The module will be flashed into the inactive vendor_boot slot (not available for devices with A-only partitioning).
    #      `boot`: The module will be flashed into the inactive boot slot (not available for devices with A-only partitioning).

WHERE_TO_INJECT=auto

#----------------
    #  `magisk`: Specify the Magisk version to install or leave the field blank.
    #      Available versions: 
    #                       Magisk-Delta-v26.4
    #                       Magisk-Delta-v27.0
    #                       Magisk-kitsune-v27-R65C33E4F
    #                       Magisk-v26.4-kitsune-2
    #                       Magisk-v26.4
    #                       Magisk-v27.0
    #      Example: magisk Magisk-v27.0
    #      To install Magisk from the same directory as neo.zip, add the "EXT:" prefix, for example, "EXT:Magisk-v24.3.zip".

INSTALL_MAGISK=

word1="[Volume(+)]"
word2="[Volume(-)]"
word3="Deleting markup"
word4="Updating sections"
word5="The script could not determine the boot slot. Choose the boot slot manually."
word6="Partition markup"
word7="Failed to markup"
word8="Substitute /proc/bootconfig with new values."
word9="Substitute /proc/cmdline with new values."
word10="Switch boot slot to"
word11="Install a patch to hide the lack of encryption?"
word12="Will only work if Magisk, KSU, or Selinux are set to Permissive mode."
word13="Install built-in safety net fix?"
word14="Do wipe data? Will erase all firmware data, internal memory will not be touched."
word15="Remove lock screen data?"
word16="Connect modified fstab during early partition mounting?"
word17="Needed mainly if you used additional dfe_patterns keys for system partitions or used the -v key to remove overlays."
word18="Remove system integrity check?"
word19="This option patches vbmeta and system_vbmeta, thus disabling system integrity check. Enable this option if you experience bootloop or if you know why it's needed, otherwise just leave it untouched."
word20="Force enable zygisk during boot?"
word21="Option will work only if Maghisk is enabled."
word22="Which forced startup mode to use?"
word23="Permanent - meaning it will be enabled every time the system starts."
word24="One-time - meaning it will be enabled only on the first system startup, thereafter forced startup will be ignored."
word25="Force record denylist during boot?"
word26="Option will work only if zygisk is enabled."
word27="Mounting vendor"
word28="dm block vendor"
word29="Vendor is located in a separate block"
word30="Vendor block"
word31="Unmounting vendor"
word32="Mounting vendor to temporary folder"
word33="Deletion completed"
word34="Unpacking"
word35="Compressing ramdisk... Decompression"
word36="Patching"
word37="Adding inject_neo mount line for A/B device"
word38="Adding inject_neo mount line for A-only device"
word39="Adding inject_neo mount line for block"
word40="Adding modified"
word41="Packing ramdisk back into"
word42="Packing"
word43="Writing"
word44="Searching for installed DFE-NEO"
word45="Searching for neo_inject in"
word46="Searching for neo_inject in super"
word47="DFE-NEO installed detected, remove or reinstall?"
word48="Configuration summary"
word49="Language"
word50="Place for inject.img"
word51="Firmware image will be sequentially in one of"
word52="Mounting fstab in early_mount"
word53="Hide unencryptedness"
word54="Zygisk on startup"
word55="Adding denylist"
word56="Installing Magisk"
word57="Magisk: Do not install"
word58="Clearing data"
word59="Delete lock data"
word60="fstab patching patterns"
word61="Continue installation with current parameters?"
word62="Adding necessary files to inject_neo"
word63="Adding new entries to"
word64="Searching for required fstab"
word65="Required fstab found"
word66="Copying fstab to inject_neo"
word67="Patching fstab in"
word68="Checking"
word69="Mounting point detected"
word70="Replacement completed"
word71="Flag removed"
word72="There is enough space in super to write neo_inject.img"
word73="There is not enough space in super to write neo_inject.img"
word74="Required"
word75="Checking neo_inject mount from new location"
word76="Attempting to compress neo_inject.img"
word77="Marking neo_inject with size"
word78="Failed to mount created partition"
word79="Failed to find created partition"
word80="Failed to create partition"
word81="Disabling system integrity check"
word82="Removing lock screen presence record"
word83="Clearing /data partition, except /data/media"
word84="Success writing neo_inject to"
word85="Determining default variables"
word86="Script launched from"
word87="Reading configuration"
word88="The archive name contains extconfig. Will attempt to read the config from the same folder where the installation archive is located."
word89="External config not found. Reading from built-in."
word90="Built-in config not found"
word91="Exit"
word92="Built-in config not found"
word93="Config detected"
word94="Checking arguments for compliance"
word95="Issues detected"
word96="All good!"
word97="Reading props and defining variables"
word98="A/B devices"
word99="Current slot"
word100="A-only device"
word101="Checking for super partition presence"
word102="Super partition not found"
word103="Super partition found at path"
word104="Searching for recovery partition"
word105="Recovery partition found. Will be easy"
word106="Recovery partition not found, will be harder"
word107="Searching for vendor_boot partition"
word108="Vendor_boot partition found. Will be easy"
word109="Vendor_boot partition not found, will be even harder"
word110="Vendor_boot partition not found, will be harder"
word111="Error in determining system update status, binary cannot be executed"
word112="User clarification required, install into current firmware?"
word113="Update installation completion detected, DFE-NEO installation will be performed into the new firmware"
word114="But before we start, I need the update to be fully installed, I cannot check this yet"
word115="Installing into slot"
word116="Completed"
word117="Format the userdata partition if you are using DFE for the first time and your /data is encrypted"
word118="Pressed not recognized"
word119="Script couldn't determine the boot slot. In force_start=true mode, installation is not available"
word120="Something went wrong with the markup, not found"
word121="Failed to markup partitions after OTA"
word122="Vendor not found"
word123="Failed to mount vendor, no more installation options after OTA"
word124="Failed to mount vendor"
word125="Device not supported"
word126="None of the fstab found in"
word127="Something went wrong during patching"
word128="Device has no super partition, use another where_to_inject parameter"
word129="Device must be A-B. Use where_to_inject with another super or auto parameter"
word130="Device has no vendor_boot block or A-only device. Use where_to_inject with another parameter"
word131="Device not supported at all"
word132="Config problem"
word133="The current version of DFE-NEO does not support A-only and devices without super partition simultaneously"
word134="Error in checking system update status, continue with force_start=true function"
word135="Firmware in update state, wait for firmware installation!"
word136="Wait for full firmware installation before running the script"
word137="Unknown system update status"
word138="Failed to write image to super, critical for A-only. Exit"
word139="Failed to write inject_neo anywhere"
word140="Failed to find in ramdisk boot/vendor_boot:"
word141="Failed to create neo_inject.img partition"
word142="Select slot _a"
word143="Select slot _b"
word144="Yes, 'install'"
word145="No, 'do not install'"
word146="Yes, 'remove'"
word147="No, 'leave it!'"
word148="Yes, 'connect'"
word149="No, 'no need'"
word150="Yes, 'disable'"
word151="Yes, 'enable'"
word152="No, 'don't do it'"
word153="'Permanent'"
word154="'One-time'"
word155="Reinstall"
word156="Remove"
word157="Yes"
word158="Exit"
word159="Current system"
word160="Exit"
word161="Ready to reboot"
word162="Installation in progress"
word163=""
word164=""
word165=""
word166=""
word167=""
word168=""
word169=""
word170=""
word171=""
word172=""
word173=""
word174=""
word175=""
word176=""
word177=""
word178=""
word179=""
word180=""
word181=""
word182=""
word183=""
word184=""
word185=""
word186=""
word187=""
word188=""
word189=""
word190=""
word191=""
word192=""
word193=""
word194=""
word195=""
word196=""
word197=""
word198=""
word199=""
word200=""
word201=""
word202=""
word203=""
word204=""
word205=""
word206=""
word207=""
word208=""
word209=""
word210=""
word211=""
word212=""
word213=""
word214=""
word215=""
word216=""
word217=""
word218=""
word219=""
word220=""
word221=""
word222=""
word223=""
word224=""
word225=""
word226=""
word227=""
word228=""
word229=""
word230=""
word231=""
word232=""
word233=""
word234=""
word235=""
word236=""
word237=""
word238=""
word239=""
word240=""
word241=""
word242=""
word243=""
word244=""
word245=""
word246=""
word247=""
word248=""
word249=""
word250=""
