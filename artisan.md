## Artisan CLI

- [မိတ်ဆက်](#introduction)
- [အသုံးပြုပုံ](#အသုံးပြုပုံ)

<a name="introduction"></a>
## မိတ်ဆက်

Artisan သည် laravel တွင်ပါဝင်သည့် command line interface တစ်ခုဖြစ်သည်။ ၄င်းတွင် application develop ပြုလုပ်ရာတွင် အလွန်အသုံးဝင်လှသော commands များစွာပါဝင်သည်။ ၄င်းမှာ Symfony ၏ console component မှ ဆင်းသက်လာခြင်း ဖြစ်သည်။


<a name="usage"></a>
## အသုံးပြုပုံ

#### အသုံးပြုနိုင်သည့် Command များကို စီရီပြသခြင်း

Artisan တွင်ပါဝင်သည့် Command အကုန်လုံးကို list အနေဖြင့် ပြသလိုပါက `list` ဟုသည့် command ကို အသုံးပြုနိုင်သည်။

	php artisan list

#### Command  တစ်ခုချင်း ၏ အသုံးပြုခြင်း လမ်းညွန်မှုကို ကြည့်ရှုခြင်း 

Command တိုင်းလိုလို တွင် “help” ဟု အပိုဆောင်း ရိုက်ခြင်း ဖြင့် မိမိတို့ အသုံးပြုမည့် Command တွင် ထည့်သွင်းရမည့် arguments များနှင့် ရွေးချယ်စရာများကို သိရှိနိုင်ပါသည်။

	php artisan help migrate

#### Configuration Environment ကို သတ်မှတ်ခြင်း

မိမိတို့ အသုံးပြုလိုသည် Configuration Environment ကို —env ဟုသော  switch ဖြင့် ရွေးချယ်သတ်မှတ်နိုင်သည်။ 


	php artisan migrate --env=local

#### လက်ရှိ အသုံးပြုနေသည့် Laravel version ကိုဖော်ပြခြင်း

မိမိတို့ လက်ရှိအသုံးပြုနေသည့် Laravel version ကို သိရှိလိုပါက —switch ကို အသုံးပြုပြီး သိရှိနိုင်ပါသည်။ 
 

	php artisan --version
