# راه اندازی نود Eigen Layer
قبل از هر چیزی میخوام بگم تو این آموزش یکم سعی میکنیم یکم Best parctice هارو رعابت کنیم. اکثر آموزش های راه اندازی نود که پیدا میشه با کاربر root انجام میشه که فوق العاده کار خطرناکیه. همچنین به استفاده از SSH بجای پسورد برای ورود به سرور هم می‌پردازیم.   

از هر جایی که براتون مقدوره میتونید سرور بخرید.
اگه [با لینک من از Hetzner خرید کنید](https://hetzner.cloud/?ref=aYjzvaVIKkSB) بهتون 20 یورو اعتبار میده    

https://hetzner.cloud/?ref=aYjzvaVIKkSB

اگه [با لینک من از Vultr ثبت نام کنید](https://www.vultr.com/?ref=9567312-8H) . بهتون ۱۰۰ دلار اعتبار میده برای ۱۴ روز. نکته ای در Vultr مهمه اینه که پرداخت کریپتو هم داره ولی باید یکبار یه نفر بهش Credit card وصل کنه.     

https://www.vultr.com/?ref=9567312-8H

 اگه نمیتونید جز AVS باشید و فقط میخواید که اسمتون توی Operator ها ثبت بشه یه سرور ارزون ۵ دلاری بگیرید. چرا میگم سرور ارزون؟ این مصرف منابع نودی که AVS نیست روی پایین رده ترین سرور Vultr هستش. تصمیم با خودتون     

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/677cdbd2-4881-4169-b01b-2300a03936b9)

---
## استفاده از کلید SSH بجای پسورد (مرحله دلخواه)
**این مرحله اجباری نیست میتونید ردش کنید. ولی بهتره که یکبار برای همیشه یاد بگیرید**    

همیشه برای وارد شدن به سرور از کلید SSH استفاده کنید بجای ورود با رمز. چیز پیچیده ای نیست. اگه ویندوز دارید توی استارت سرچ کنید Power Shell . این یه کامند لاینه که بعضی از دستورات لینوکسی هم میتونید داخلش استفاده کنید.      
با اجرا کردن این دستور یه جفت کلید rsa براتون ساخته میشه. اگر براش پسورد ست کنید بهتره تا اگه کلیدتون دزدیده شد کسی نتونه به سرور هاتون دسترسی پیدا کنه   

```shell
ssh-keygen
```
![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/1db5cb57-b446-44b1-b9a2-b07e69232ba3)


مسیری که کلید داخلش ساخته شده رو توی ترمینال مینویسه. برید توی اون پوشه ۲ تا فایل دارید. یدونه `id_rsa` و `id_rsa.pub`   

اولی کلید خصوصی سرور شماس که باید ازش پشتیبان بگیرید (مثل seed phrase ) که مهمه و هیچ کس نباید بهش دسترسی داشته باشه. اونی که آخرش pub نوشته کلید عمومی هستش. هر جای درست حسابی که سرور بگیرید موقع ساخت یه بخشی داره برای اضافه کردن کلید عمومی که باید محتوای این فایل رو کامل کپی کنید. دقت کنید سر و تهش فاصله یا کارکتر اضافی کپی نکنید. مثلا توی vultr موقع ساحت سرور این شکلیه   

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/91b877d1-28b8-4b9a-bd74-13f6ba8ce293)

---
## ساخت سرور

برای این آموزش یه سرور لینوکسی با سیستم عامل Ubuntu version 22 بگیرید. بعضی جاها موقع ساخت سرور میتونید یه اپلیکشن هم بگید پیش فرض نصب باشه. بازم توی Vultr داکر رو داره. اگه اینو انتخاب کنید یه مرحله کارتون راحت تره و دیگه لازم نیست Docker نصب کنید. بدین شکل   

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/0480a971-53fc-4124-861d-4e513d48c7e9)

به هر حال من آموزش رو با نصب داکر پیش می‌برم. موقع ساخت حواستون باشه SSH Key که آپلود کردید رو انتخاب کنید (اگه مرحله ساخت SSH رو رد کردید خالی بزارید) 
سرور من ساخته شد.    

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/a4720a49-fe79-4106-9db8-4bba48bf0e08)

با این دستور لاگین میکنیم.اگه از کلید SSH استفاده کرده باشید دیگه موقع ورود رمز نمیخواد      

```shell
ssh root@209.250.241.179
```
در اولین ورود یه سوال در مورد فینگر پرینت میپرسه که yes میزنیم و وارد سرور میشیم.   

## ساخت کاربر با دسترسی sudo
همونطور که اول آموزش گفتم باید یه کاربری بجز root بسازیم و با اون کار کنیم. هر جا arash دیدی با نام کاربری خودتون عوضش کنید   

```shell
adduser arash
```
یه رمز نسبتا پیچیده بزنید و ولی کوتاه بزنید (چون باید مدام تایپش کنید) بقیه موارد رو هم اینتر بزنید بره   

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/8dc3d0a9-c068-45b8-b063-6414b6906e3c)

به گروه sudoer ها اضافه اش کنید و سوییچ کنید به کاربری که ساختید و تستش کنید (خط به خط بزنید)   

```shell
usermod -aG sudo arash

su - arash

sudo whoami
```
اگه بعد از اچرای خط آخر و وارد کردن پسورد خروچی نوشت root یعنی درست انجام شده   

## فعال کردن SSH برای کاربر جدید (مرحله دلخواه)
اگه مرحله ساخت کلید SSH رو رد نکردید، این مرحله رو هم باید انجام بدید.      
اول با این دستور فایل مورد نظر رو میسازیم   
```shell
mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh
```

حالا باید کلید عمومی رو تو این فایل بنویسیم. راه زیاده. مثلا با ادیتور nano متیونید ویرایشش کنید   
```shell
nano ~/.ssh/authorized_keys
```

 خط اول کلید عمومی تون رو بهش paste کنید (با فشار دادن ctrl+shift+v) و با فشار دادن ctrl+x و زدن y و سپس اینتر ازش خارج بشید   
 حالا یه ترمینال دیگه باز کنید و با یوزر جدید باید بتونید وارد سرور بشید. بدون پسورد   
 
```shell
ssh arash@209.250.241.179 
```

اگه تا اینجا پیش اومدی و میخوای از امنیت سرورت مطمئن بشی پیشنهاد میکنم [این لینک](https://www.digitalocean.com/community/tutorials/how-to-harden-openssh-on-ubuntu-20-04) رو بخونی و موارد Step -1 رو انجام بدی.  



## نصب داکر
خیلی مختصر بگم داکر یه تکنولوژیه مثل Virtual Machine که متیونی داخل سیستم عامل یه کامپیوتر مجازی بسازی. منتها خیلی خیلی بهینه تر و خیلی فرق میکنه با VM . کار اصلیش اینه برنامه هارو ایزوله میکنه و محیط اجرایی یکسان فراهم میکنه براشون.      
برای نصبش از دستورات [داکیومنت رسمی](https://docs.docker.com/engine/install/ubuntu/) استفاده میکنیم
۱. اضافه کردن داکر به ریپازیتوری apt
```shel
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
۲. نصب داکر و داکر کامپوز
```shell

 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```
۳. تست داکر
```shell

sudo docker run hello-world

```
اگه همچین پیامی رو دیدید یعنی داکر بدرستی نصب شده 
> Hello from Docker!
> This message shows that your installation appears to be working correctly.

#### کار کردن با docker بدون sudo
۱. دستورات زیر رو وارد کنید
```shell
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```
۲. حالا باید بتونید بدون sudo دستورات داکر رو اجرا کنید.مثلا با اجرای این دستور باید همون پیام قبلی رو ببینید
```shell
docker run hello-world
```

## نصب eigenlayer CLI

راحتترین روش استفاده از binary های موجود در گیت هاب خود پروژه اس   

مطمئن بشید در مسیر Home قرار دارید. با این دستور همیشه به مسیر Home کاربری که باهاش لاگین هستید وارد میشیم    

```shell
cd ~
```
باینری رو دانلود میکنیم و برای دسترسی آسان در PATH قرار میدیم   
```shell
curl -sSfL https://raw.githubusercontent.com/layr-labs/eigenlayer-cli/master/scripts/install.sh | sh -s
export PATH=$PATH:~/bin
```

## ساخت کلید های ECDSA و BLS

بجای [keyname] نام مورد دلخواه خودتون رو قرار بدید. بعد از اجرای هر کدوم و وارد کردن یه رمز مناسب **حتما از Private key بک آپ بگیرید توی یه جای مطمئن** همونطور که از کلید خصوصی کیف پولتون محافظت میکنید.
```shell
eigenlayeroperator keys create --key-type ecdsa [keyname]
eigenlayeroperator keys create --key-type bls [keyname]
```

توصیه میکنم از کل خروجی این دستورات یه بک آپ بگیرید و یجایی ذخیره کنید. همجنین محتوای فایل هایی که در مسیر  Key Location مشخص شده رو با دستور زیر روی صفحه به نمایش در بیارید و هر کدوم رو تو یه فایل رو سیستم خودتون بک آپ بگیرید. در اینجا اسم کلید من arash هست که برای شما میشه نامی که انتخاب کردید   

```shell
cat /home/arash/.eigenlayer/operator_keys/arash.ecdsa.key.json
cat /home/arash/.eigenlayer/operator_keys/arash.bls.key.json
```

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/f550e0ce-4ecf-44c9-8e8c-beceab5125f3)


# ثبت Operator

#### آپلود لوگو و متا دیتا
اول از همه یه لوگو با فرمت png احتیاج دارید که باید در یه لینک عمومی قابل دسترس باشه. یه راه رایگان اینه که از آپلود مدیای گیت هاب (سو)استفاده کنیم. باید داخل گیت‌هاب لاگین باشید. یه ریپازیتوری انتخاب میکنید، میتونید ریپازیتوری خودتون رو بسازید. از بالای صفحه میرید داخل تب Issues و روی دکمه New Issue میزنید.   
فایل مورد نظر و Drag & drop میکنید روی قسمت Add your description here... چند ثانیه صبر کنید و فایل آپلود میشه یه لینک میده.   
لینکش رو توی تب جدید باز میکنید.   
یه آدرس طولانی میده.   
از اول آدرس تا اونجایی که به .png ختم میشه رو یجا ذخیره کنید که لازمش داریم. این میشه لینک عمومی به فایل شما.   
**مهم! لازم نیست که روی Submit new issue کلیک کنید. بعد از اینکه لینک رو کپی کردید صفحه رو ببندید وگرنه ممکنه abuse حساب بشه و اکانت گیت هاب بسته بشه**   



![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/e1843830-11df-4320-aa17-faa6352036a1)

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/246c69b9-6db5-4e79-9478-c6c52d64f9ca)


حالا وارد [این لینک](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/pkg/operator/config/metadata-example.json) بشید و فایل metadata-example.json رو دانلود کنید و اسمش رو تغییر بدید به metadata.json و محتوای داخلش رو به تغییر بدید. اسم هر چیزی میتونه باشه. بقیه فیلد ها مشخصه. برای لوگو از لینکی که در مرحله قبل بدست آوردیم استفاده کنید. حتما باید لینک به .png ختم بشه وگرنه خطا میده.      
بعد از ذخیره فایل دوباره به همون روش لوگو، اینبار فایل metadata.json رو آپلود و لینکش رو ذخیره کنید. ( لینک به metadata.json ختم میشه)   


#### ساخت فایل operator.yaml

فایل نمونه رو از [این لینک](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/pkg/operator/config/operator-config-example.yaml) دانلود کنید


**مهم! فایل های YAML به space و tab حساس هستند. یه space اضافی میتونه فایل رو خراب کنه. پس در ویرایش این فایل ها دقت کنید و سعی کنید از ادیتور مناسب استفاده کنید**   
فیلد های زیر رو پر کنید. در انتها تصویر نمونه رو میزارم.   

- فیلد address میشه همون آدرس ECDSA که موقع ساخت کلید جنریت شد
- فیلد earnings_receiver_address میتونه همون آدرس بالایی یا یکی از والت های خودتون باشه
- فیلد metadata_url میشه آدرسی که از آپلود فایل metadata.json بدست اومد
- فیلد el_slasher_address توسط تیم مشخص شده توی داکیومنت `0xD11d60b669Ecf7bE10329726043B3ac07B380C22`
- فیلد bls_public_key_compendium_address توسط تیم مشخص شده توی داکیومنت `0xc81d3963087Fe09316cd1E032457989C7aC91b19`
- فیلد eth_rpc_url باید یه rpc پابلیک واسه شبکه goerli باشه. از [این لینک](https://chainlist.org/chain/5) یکی رو انتخاب کنید
- فیلد private_key_store_path میشه مسیر کلید فایل ecdsa که تو مرحله ساخت کلید نوشته شد
- فیلد bls_private_key_store_path میشه مسیر کلید فایل bls که تو مرحله ساخت کلید نوشته شد

 داخل سرور با این دستور یه فایل تو ادیتور نانو باز کنید و محتوای ویرایش شده داخلش paste کنید و با فشار دادن Ctrl+X و وارد کردن y و سپس enter ذخیره اش کنید
```shell
nano operator.yaml
```
نمونه فایل من:   

![image](https://github.com/0x4r45h/EigenLayer-node/assets/19164358/3d74fea3-7ef7-41bd-a24d-37683dc924f3)


#### ارسال اتر Goerli به آدرس اپراتور و درخواست ثبت On Chain

یه مقدار کم اتریوم Goerli به آدرس اپراتور بفرستید و با این دستور درخواست ثبت اپراتور رو ارسال کنید. پسورد هایی که در حین ساخت کلید ها وارد کردید رو الان ازتون میخواد   

```shell
eigenlayeroperator register operator.yaml
```
با این دستور وضعیت ثبت اپراتور رو بررسی کنید   

```shell
eigenlayeroperator status operator.yaml
```


اگه پیام Operator is registered رو دیدید یعنی اپراتور بدرستی ثبت شده.   
بعد از چند دقیقه [تو این لیست](https://goerli.eigenlayer.xyz/operator) با سرچ آدرس یا اسم اپراتور میتونید پیداش کنید و بهش دلیگیت کنید   


# راه اندازی Node جهت فعالیت به عنوان AVS   

تا اینجا ما فقط اپراتور رو ثبت کردیم. سوالی که پیش میاد اینه آیا اصلا فقط برای ثبت اپراتور به سرور احتیاج داریم یا نه ؟ خودم هم هنوز جوابش رو نمیدونم. به هر حال برای شروع Node خودمون مراحل زیر رو پیش میریم.  

ابتدا ریپازیتوری زیر رو کلون کنید و واردش بشید   

```shell
cd ~

git clone https://github.com/Layr-Labs/eigenda-operator-setup.git

cd eigenda-operator-setup
```
فایل env. رو با nano باز کنید و فیلدهای زیر رو با مقادیر خودتون آپدیت کنید
```shell
nano .env
```
- فیلد NODE_HOSTNAME میشه IP سرور خودتون
- فیلد NODE_CHAIN_RPC باید یه RPC شبکه Goerli باشه، میتونید همونی که برای اپراتور استفاده کردید رو اینجا هم بزارید
- فیلد USER_HOME رو بجای ubuntu باید نام کاربری که توی لینوکس ساختید بزارید. برای من میشه `home/arash/`
- فیلد NODE_ECDSA_KEY_FILE_HOST مسیر کلید ECDSA هستش. کافیه بجای opr اسم کلید خودتون رو بزارید. برای من میشه arash
- فیلد NODE_BLS_KEY_FILE_HOST مسیر کلید BLS هستش. کافیه بجای opr اسم کلید خودتون رو بزارید. برای من میشه arash
- فیلد NODE_ECDSA_KEY_PASSWORD باید رمز کلید ECDSA باشه
- فیلد NODE_BLS_KEY_PASSWORD باید رمز کلید BLS باشه

ذخیره و خروج از نانو فراموش نشه     

فولدرهای مورد نیاز رو بسازید   
```shell
mkdir -p $HOME/.eigenlayer/eigenda/logs
mkdir -p $HOME/.eigenlayer/eigenda/db
```

### درخواست عضویت در EigenDA
اینجا باید به مقداری TVL گرفته باشید که جز 60 نفر اول باشید وگرنه failed میشه. به هر حال ما میزنیم و fail شدنمون رو تماشا میکنیم   

```shell
./run.sh opt-in
```

در نهایت با دستور زیر نود رو اجرا میکنیم    
```shell
docker compose up -d
```
و بدین روش لاگ هارو چک میکنیم   
```shell
docker compose logs -f
```
برای اینکه مطمئن بشید پورت بازه همینجوری که لاگ رو چک میکنید یه ترمینال دیگه روی سیستم خودتون باز کنید با این دستور یه درخواست به node بدید. بجای SERVER_IP طبیعتا باید IP سرورتون رو بزارید. اگه پورت باز باشه یکسری لاگ میبینید که روی صفحه میافته (ممکنه بخاطر فیلترینگ درخواست به سرور نرسه. پس با vpn هم امتحان کنید)   

```shell
curl SERVER_IP:32004
```

تبریک میگم شما نود خودتون رو راه اندازی کردید!   

**اگه براتون مفید بود به این ریپازیتوری ستاره و بدید**   

اگه براتون مقدور بود به Node من [در این آدرس](https://goerli.eigenlayer.xyz/operator/0xe5619c950ce9d488be504d8818d668ef908af307) delegate کنید    

https://goerli.eigenlayer.xyz/operator/0xe5619c950ce9d488be504d8818d668ef908af307



سوالاتتون در Issue های همین پروژه بپرسید که بقیه هم ببینند و استفاده کنند.   

با من در [توییتر](https://twitter.com/4r45h1)https://twitter.com/4r45h1 در ارتباط باشید









