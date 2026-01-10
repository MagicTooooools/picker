---
title: 基于QEMU测试Loongarch龙架构
url: https://blog.xulihang.me/trying-loongarch-with-qemu/
source: xulihang's blog
date: 2026-01-09
fetch_date: 2026-01-10T03:35:38.261355
---

# 基于QEMU测试Loongarch龙架构

# еҹәдәҺQEMUжөӢиҜ•Loongarchйҫҷжһ¶жһ„

[йҰ–йЎө](/ "зҪ‘з«ҷйҰ–йЎө")
[еҲҶзұ»](/categories/ "ж–Үз« еҲҶзұ»")
[ж Үзӯҫ](/tags/ "ж Үзӯҫзҙўеј•")
[й“ҫжҺҘ](/links/ "еҸӢжғ…й“ҫжҺҘ")
[з•ҷиЁҖ](/guestbook/ "з•ҷиЁҖдәӨжөҒ")
[жҗңзҙў](/search/ "е…Ёз«ҷжҗңзҙў")
[е…ідәҺ](/about/ "е…ідәҺз«ҷй•ҝ")
[и®ўйҳ…](/feed/ "з§Қеӯҗи®ўйҳ…")

2026-01-09
|
еҲҶзұ»
[жҠҖжңҜйҡҸз¬”](/categories/#жҠҖжңҜйҡҸз¬” "жҠҖжңҜйҡҸз¬”")

жңҖиҝ‘жңүдёӘе…¬еҸёе®ўжҲ·иҜҙиҰҒеңЁ3A6000дёҠиҝҗиЎҢжҲ‘д»¬зҡ„дә§е“ҒгҖӮеӨ§дёҖзҡ„ж—¶еҖҷ279е…ғд№°дәҶеҸ°йҫҷиҠҜ2Fзҡ„йҖёзҸ‘8089dз¬”и®°жң¬пјҢйӮЈж—¶иҝҳжҳҜmipsжһ¶жһ„зҡ„пјҢзҺ°еңЁйҫҷиҠҜе·Із»ҸдҪҝз”Ёж–°зҡ„Loongarchйҫҷжһ¶жһ„дәҶгҖӮ

жҲ‘зңӢдәҶдёӢзҪ‘дёҠжІЎжңүеҫҲдҫҝе®ңзҡ„ж•ҙжңәеҸҜд»Ҙд№°еҲ°пјҢдәҺжҳҜе°ұиҖғиҷ‘з”ЁQEMUжқҘжЁЎжӢҹжөӢиҜ•зҺҜеўғгҖӮ

QEMUжңүдёӨз§ҚжЁЎејҸпјҢдёҖз§ҚжҳҜsystemпјҢеҸҜд»ҘжЁЎжӢҹж•ҙдёӘзЎ¬д»¶зҺҜеўғпјҢдёҖз§ҚжҳҜuserпјҢеҸҜд»ҘзӣҙжҺҘеңЁиҮӘе·ұзҡ„зҺҜеўғдёӯжү§иЎҢеҸҰдёҖдёӘжһ¶жһ„зҡ„зЁӢеәҸгҖӮ

## SystemжЁЎејҸ

жҲ‘д»¬еҸҜд»ҘзӣҙжҺҘдёӢиҪҪйҫҷиҠҜе®ҳж–№зҡ„Loongnixзі»з»ҹзҡ„qemuзЈҒзӣҳй•ңеғҸжқҘеҗҜеҠЁдёҖдёӘLoongarchзҡ„зі»з»ҹгҖӮ

й•ңеғҸй“ҫжҺҘпјҡ<https://pkg.loongnix.cn/loongnix/20/isos/Loongnix-20.7.rc1/Loongnix-20.7.rc1.cartoon.gui.loongarch64.en.qcow2>

EFIеӣәд»¶пјҡ<https://github.com/loongson/Firmware/blob/main/LoongArchVirtMachine/QEMU_EFI.fd>

з„¶еҗҺз”Ёд»ҘдёӢе‘Ҫд»ӨеҗҜеҠЁпјҡ

```
qemu-system-loongarch64 -m 4G -cpu la464-loongarch-cpu -machine virt -smp 4 -bios QEMU_EFI.fd -serial stdio -device virtio-gpu-pci -net nic -net user -device nec-usb-xhci,id=xhci,addr=0x1b -device usb-tablet,id=tablet,bus=xhci.0,port=1 -device usb-kbd,id=keyboard,bus=xhci.0,port=2 -hda Loongnix-20.7.rc1.cartoon.gui.loongarch64.en.qcow2
```

еҜҶз ҒжҳҜLoongson20гҖӮ

## UserжЁЎејҸ

жҲ‘д»¬йңҖиҰҒжңүдёҖдёӘеҹәжң¬зҡ„rootfsпјҢеҸҜд»Ҙз”Ёdebootstrapжһ„е»әжҲ–иҖ…и§ЈеҺӢзҺ°жҲҗжү“еҢ…еҘҪзҡ„зүҲжң¬пјҲ[CLFS-for-LoongArch](https://github.com/sunhaiyong1978/CLFS-for-LoongArch)пјүгҖӮ

дёӢйқўжҳҜдҪҝз”Ёdebootstrapзҡ„ж–№жі•пјҢжөӢиҜ•зҺҜеўғжҳҜDebian 14 Testingпјҡ

```
sudo apt install debootstrap qemu-user-binfmt binfmt-support debian-ports-archive-keyring
mkdir loongson
sudo debootstrap --arch=loong64 --foreign sid loongson http://mirrors.ustc.edu.cn/debian
sudo cp /usr/bin/qemu-loongarch64 loongson/usr/bin/
sudo mount -t proc proc loongson/proc
sudo mount -t sysfs sys loongson/sys
sudo mount -t devpts devpts loongson/dev/pts
sudo chroot loongson /debootstrap/debootstrap --second-stage
```

## ж–°ж—§дё–з•Ң

LoongArchжңүдёӨеҘ—дёҚе…је®№зҡ„иҪҜд»¶дҪ“зі»пјҢеҸ«еҒҡж—§дё–з•Ңе’Ңж–°дё–з•ҢпјҢе®ҳж–№еҸ«ABI 1.0е’ҢABI 2.0гҖӮ

дё»иҰҒжҳҜж—©жңҹе®ҳж–№з»ҙжҠӨдәҶдёҖеҘ—иҮӘе·ұзҡ„е·Ҙе…·й“ҫпјҢжҺҘиҝ‘MIPSзҡ„йЈҺж јпјҢеҗҺйқўжҺҘеҸ—зӨҫеҢәзҡ„еҸҚйҰҲпјҢиһҚе…ҘдәҶејҖжәҗзӨҫеҢәгҖӮ

дёҖдёӘжҜ”иҫғжҳҺжҳҫзҡ„еҢәеҲ«жҳҜж–°дё–з•Ңзҡ„зЁӢеәҸи§ЈйҮҠеҷЁи·Ҝеҫ„жҳҜ /lib64/ld-linux-loongarch-lp64d.so.1пјҢиҖҢж—§дё–з•Ңзҡ„зЁӢеәҸи§ЈйҮҠеҷЁи·Ҝеҫ„жҳҜ /lib64/ld.so.1гҖӮ

дёҖиҲ¬жҜ”иҫғиҖҒзҡ„Linux 4.xеҶ…ж ёзҡ„пјҢйә’йәҹгҖҒUOSгҖҒLoongnix 20иҝҷдәӣжҳҜж—§дё–з•Ңзҡ„зі»з»ҹпјҢзӨҫеҢәзҡ„DebianгҖҒArchе’ҢLoongnix 25жҳҜж–°дё–з•Ңзі»з»ҹгҖӮ

ж–°дё–з•Ңзі»з»ҹеҸҜд»ҘйҖҡиҝҮliblolе…је®№ж—§дё–з•Ңзі»з»ҹзҡ„иҪҜд»¶гҖӮдёҚиҝҮж—§дё–з•Ңе…је®№ж–°дё–з•Ңе°ұжІЎжңүйӮЈд№Ҳе®№жҳ“дәҶгҖӮ

иҰҒзј–иҜ‘ж”ҜжҢҒжҹҗдёӘдё–з•Ңзі»з»ҹзҡ„зЁӢеәҸпјҢжңҖеҘҪжҳҜеңЁзӣ®ж Үзі»з»ҹзј–иҜ‘пјҢжҲ–иҖ…дҪҝз”ЁеҜ№еә”зҡ„дәӨеҸүзј–иҜ‘е·Ҙе…·й“ҫгҖӮ

## зӣёе…ій“ҫжҺҘ

* [иҜ·ж•ҷеңЁx86е№іеҸ°дҪҝз”ЁqemuиҝҗиЎҢе•ҶдёҡзүҲпјҲж—§дё–з•Ңпјүloongarch64зҡ„ж–№жі•](https://github.com/sunhaiyong1978/CLFS-for-LoongArch/issues/53)
* [ж—§дё–з•ҢдёҺж–°дё–з•Ң](https://areweloongyet.com/docs/old-and-new-worlds)
* [еҹәдәҺKVMзҡ„ж–°дё–з•ҢиҝҗиЎҢж—§дё–з•Ңй—ӯжәҗиҪҜд»¶](https://bbs.loongarch.org/d/335-kvm)

[дёҠдёҖзҜҮ](/change-a-person/)

дёӢдёҖзҜҮ

Please enable JavaScript to view the [comments powered by Disqus.](https://disqus.com/?ref_noscript)

[Po](http://github.com/xulihang/xulihang.github.io/new/master/_posts "ж’°еҶҷж–Үз« ")wer[ed](http://github.com/xulihang/xulihang.github.io/edit/master/_posts/2026-01-09-trying-loongarch-with-qemu.md "зј–иҫ‘йЎөйқў") by [Jekyll](http://jekyllrb.com) @ [GitHub](http://github.com/xulihang/xulihang.github.io "йЎ№зӣ®дё»йЎө")
| [В©](http://creativecommons.org/licenses/by-sa/3.0/cn/ "и®ёеҸҜеҚҸи®®") 2013 - 2026 [xulihang](/about/)