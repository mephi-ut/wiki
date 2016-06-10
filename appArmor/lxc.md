To define a separate profile for some program in a container you may use "cx"
```
/path/to/program cx,
profile /path/to/program {
    …
}
```
see details here: http://wiki.apparmor.net/index.php/QuickProfileLanguage#File_permissions

To forbid a way to see "dmesg" via "/proc/kmsg" you may add rule:
```
deny /proc/kmsg rw,
```

result profile:
```
cat >/etc/apparmor.d/lxc/lxc-someName <<EOF
profile lxc-container-someName flags=(attach_disconnected,mediate_deleted) {
        #include <abstractions/lxc/container-base>

        deny mount fstype=devpts,
        deny /proc/kmsg rw,

        /usr/sbin/php5-fpm cx,
        /bin/ping cx,

        #/bin/init Pix,

        profile /usr/sbin/php5-fpm flags=(attach_disconnected,mediate_deleted) {
                deny mount fstype=devpts,
                deny /proc/kmsg rw,

                #include <abstractions/php5>

                /dev/null rw,
                /dev/urandom r,

                capability chown,
                capability setuid,
                capability kill,
                capability setgid,

                /usr/sbin/php5-fpm r,
                /usr/lib/** mr,
                /lib/x86_64-linux-gnu/** mr,
                /etc/localtime r,
                /var/log/php5-fpm/* w,
                /var/log/php5-fpm.log w,
                /var/www/site.mephi.ru/** r,
                /var/www/site.mephi.ru/root/upload/** rw,
                /usr/share/zoneinfo/ r,
                /usr/share/zoneinfo/** r,
                /var/lib/php5/sessions/* krw,
                /run/php5-fpm.pid w,
                /run/php5-fpm.sock w,
                /etc/php5/** r,
                /etc/passwd r,
                /etc/hosts r,
                /etc/resolv.conf r,
                /etc/host.conf r,
                /etc/ssl/openssl.cnf r,
                /etc/nsswitch.conf r,
                /etc/group r,
                /etc/ld.so.cache r,
                /etc/services r,
                /proc/sys/kernel/ngroups_max r,
                /proc/meminfo r,
        }

        profile /bin/ping flags=(attach_disconnected,mediate_deleted,complain) {
                deny mount fstype=devpts,
                deny /proc/kmsg rw,

                #include <abstractions/base>
                #include <abstractions/consoles>
                #include <abstractions/nameservice>

                capability net_raw,
                capability setuid,
                network inet raw,

                /bin/ping mixr,
                /etc/modules.conf r,
        }
}
EOF
```

To enable this profile for a container:
```
/etc/init.d/apparmor restart

cat >> /srv/lxc/someName/config <<EOF
lxc.aa_profile = lxc-container-someName
lxc.aa_allow_incomplete = 1
EOF
```