# russia mobile internet whitelist


<br>
🇷🇺 список доменов и ip-адресов, которые остаются доступными в россии, когда мобильный интернет «ограничивают».

---

### **🇺🇸 english**

#### what's the problem?

in russia, mobile internet is heavily restricted, but "restricted" doesn't even begin to describe the chaos. there are no consistent rules. the situation varies not just by region, but by your specific mobile operator, the cell tower you're connected to, and even your virtual operator (mvno). for instance, yota, which is owned by and runs on megafon's network, can have a radically different (and often stricter) whitelist than megafon itself, even if you're standing in the same spot.

here's what you can expect:
*   **total blackout:** in some regions, especially villages and rural areas where the government assumes nobody is tech-savvy, the internet is simply turned off. sometimes, literally only two or three government websites work. nothing else.
*   **brutal throttling:** in other areas, any traffic to a non-whitelisted resource is throttled down to an unusable 14 kb/s (or even lower).
*   **whitelisting in action:** your connection attempts to unapproved servers might get a `connection reset` packet, or they might just disappear into a blackhole, resulting in a `timeout`. even icmp pings are blocked.
*   **sni whitelisting:** for some lucky people, the restrictions are still basic. the operator only checks the sni (domain name) you're trying to connect to. if it's on their list, you're good. your server's ip address doesn't matter.

#### the lists (and their new purpose)

the lists are a community effort, compiled by people who scan massive ip ranges during lockdowns to see what's still alive. **important:** this list is universal, a collection of what works across different operators and regions. it's not specific to just one provider.

*   `whitelist.txt`: a list of (sub)domains (sni) that are allowed. perfect for sni spoofing in vless/trojan.
*   `ipwhitelist.txt`: this is what `cidrwhitelist.txt` used to be. a list of individual whitelisted ip addresses, one per line.
*   `cidrwhitelist.txt`: the new format. this file contains whitelisted ip subnets in cidr notation (e.g., `0.0.0.0/0`).

#### ⚠️ the reality: sni vs ip/cidr and "the shield"

this is no longer a simple problem. operators are moving to a combined `ip + sni` model. this means even if you spoof a whitelisted domain perfectly, the connection will be blocked if your server's ip isn't also on their allowlist.

there's also a strange phenomenon we call **"the shield"** (personally verified by the repo owner, only on t2/tele2 so far).
*   **what is it?** a "shield" makes you immune to whitelisting. it's not a tariff or a service you can buy. there are no "static ip" services on mobile networks. it's a secret flag in the operator's system.
*   **how do people get it?** it's not given out randomly. it's either for "special" individuals (like a local politician) or for very persistent complainers. there was a real case where a user, during a whitelist, couldn't access government services. he threatened the operator with legal action, citing specific laws. they compensated him and silently enabled "the shield" on his account, likely to prevent future trouble.
*   **does it always work?** no. during the most severe, state-mandated shutdowns, even those with the shield lose connectivity like everyone else.

#### how to survive: tools and strategies

**1. how to tell if you're under a whitelist?**
it's simple. open a terminal or a browser.
*   can you access `yandex.ru` or `gosuslugi.ru`? if yes, you have some connection.
*   can you access `2ip.ru`, `yahoo.com`, or ping `1.1.1.1`? if they all time out or get blocked, while yandex works, you are under a whitelist.

**2. recommended clients & protocols**
your best bets are **vless** or **trojan**. always use encryption (reality/tls).
*   **ios:** streisand, happ.
*   **android:** husi, v2rayng.
*   **pc (windows / linux):** throne, or nekobox (specifically the fork by **qr243vbi**). **warning: the original nekobox is abandoned and outdated, do not use it!**
*   **ipv6?** forget it. russian mobile operators are greedy and haven't invested in ipv6 infrastructure. nobody has tested whitelisting on it.

**3. your options**
1.  **sni spoofing:** the easiest method. just use a domain from `whitelist.txt`. works only if your operator isn't checking ips.
2.  **find a whitelisted server:** search for a hosting provider (they exist both inside and outside of russia) whose ip subnets are in our `ipwhitelist.txt` or `cidrwhitelist.txt`. buy a server, get an ip from the list, and you're golden.
3.  **two-server setup:** the most reliable solution. the traffic flow is: `client -> russian server (with a whitelisted ip) -> foreign server -> internet`. this is more complex and costly, but it bypasses ip-based whitelisting.

**4. join the discord server!**
this readme is just the tip of the iceberg. the discord is a chaotic but incredibly valuable living knowledge base. people are constantly helping each other, sharing working configs, discussing which mvnos are working in which regions *right now*, and even talking about bypassing censorship on wired internet. if you're serious about this, you need to be there.

> **[view whitelist.txt](./whitelist.txt)**
>
> **[view ipwhitelist.txt](./ipwhitelist.txt)**
>
> **[view cidrwhitelist.txt](./cidrwhitelist.txt)**

---

### **🇷🇺 русский**

#### в чём проблема?

мобильный интернет в россии подвергается жёстким ограничениям, но слово «ограничения» не описывает всего хаоса. единых правил нет. ситуация меняется в зависимости от региона, вашего оператора, конкретной вышки, к которой вы подключены, и даже от того, виртуальный у вас оператор (mvno) или нет. например, yota, которая принадлежит мегафону и работает на его же сетях, может иметь радикально отличающийся (и обычно более жесткий) белый список, чем сам мегафон, даже если вы стоите в одной и той же точке.

вот, с чем вы можете столкнуться:
*   **полный блэкаут:** в некоторых регионах, особенно в селах и деревнях (где правительство считает, что технически грамотных людей нет), интернет просто выключают. иногда работают буквально два-три сайта госуслуг. и всё.
*   **жесточайший троттлинг:** в других местах любой трафик на адреса не из белого списка режется до непригодных 14 кбит/с (а то и ниже).
*   **вайтлистинг в действии:** ваши попытки подключиться к «запрещенным» серверам будут либо получать в ответ пакет `connection reset`, либо просто улетать в черную дыру с ошибкой `timeout`. блокируются даже icmp-пинги.
*   **вайтлистинг по sni:** некоторым везет. у них оператор проверяет только sni (имя домена), к которому идет подключение. если домен в списке — всё работает. ip-адрес вашего сервера в этом случае не важен.

#### списки (и их новое назначение)

эти списки — результат работы всего коммьюнити. во время блокировок люди сканируют огромные диапазоны ip-адресов, чтобы найти то, что еще живо. **важно:** список универсальный, он собран с нескольких операторов и из разных регионов, а не под кого-то одного.

*   `whitelist.txt`: список (саб)доменов (sni), которые входят в белый список. идеальны для sni-спуфинга в vless/trojan.
*   `ipwhitelist.txt`: то, чем раньше был `cidrwhitelist.txt`. список отдельных ip-адресов из белого списка, по одному на строку.
*   `cidrwhitelist.txt`: новый формат. в этом файле содержатся подсети из белого списка в cidr-нотации (например, `0.0.0.0/0`).

#### ⚠️ внимание: ip/cidr-вайтлистинг и феномен «щита»

проблема усложнилась. операторы переходят на комбинированную модель `ip + sni`. это значит, что даже если вы идеально подменили домен, соединение заблокируют, если ip вашего сервера не входит в разрешенный список.

а еще есть странный феномен, который мы называем **«щит»** (проверено лично владельцем репозитория, пока только на т2/теле2).
*   **что это такое?** «щит» дает вам иммунитет к вайтлистам. это не тариф и не услуга, которую можно купить. на мобильных сетях в принципе нет услуг типа «статический ip». это секретный флажок в системе оператора.
*   **как его получают?** его не выдают просто так. либо «особым» людям (какому-нибудь депутату), либо самым настойчивым жалобщикам. был реальный случай: у человека во время вайтлиста не работали госуслуги. он начал угрожать оператору, приводя реальные законы и доказательства. в итоге оператор сделал ему компенсацию и тихонько, в своей системе, подключил «щит», чтобы в будущем не «получить по попе».
*   **он работает всегда?** нет. во время самых жестких, принудительных государственных шатдаунов интернет пропадает даже у тех, у кого есть «щит».

#### как выжить: инструменты и стратегии

**1. как понять, что вас зацепило?**
очень просто.
*   открывается `yandex.ru` или `gosuslugi.ru`? если да, какое-то соединение есть.
*   открывается `2ip.ru`, `yahoo.com` или пингуется `1.1.1.1`? если всё висит, выдает тайм-аут или ошибку, а яндекс работает — поздравляю, вы под вайтлистом.

**2. рекомендуемые клиенты и протоколы**
лучший выбор — **vless** или **trojan**. всегда используйте шифрование (reality/tls).
*   **ios:** streisand, happ.
*   **android:** husi, v2rayng.
*   **пк (windows / linux):** throne или nekobox (а именно форк от **qr243vbi**). **внимание: оригинальный nekobox заброшен и устарел, не используйте его!**
*   **а что с ipv6?** забудьте. мобильные операторы в россии жадные и не вкладывались в ipv6. вайтлистинг на нём никто не проверял.

**3. ваши варианты**
1.  **sni-спуфинг:** самый простой метод. просто используйте домен из `whitelist.txt`. сработает, только если ваш оператор еще не проверяет ip.
2.  **найти сервер в белом списке:** ищите хостинг-провайдера (они есть и в рф, и за ее пределами), чьи ip-подсети есть в наших `ipwhitelist.txt` или `cidrwhitelist.txt`. покупаете сервер, выбиваете ip из списка — и вы в дамках.
3.  **схема с двумя серверами:** самое надежное решение. схема трафика: `клиент -> российский сервер (с «белым» ip) -> зарубежный сервер -> интернет`. сложнее и дороже, но обходит ip-вайтлистинг.

**4. зайдите в дискорд сервер!**
этот readme — лишь верхушка айсберга. дискорд — это хаотичная, но невероятно ценная и живая база знаний. люди постоянно помогают друг другу, делятся рабочими конфигами, обсуждают, какой mvno и в каком регионе *прямо сейчас* работает, и даже говорят про обход блокировок на проводном интернете. если вы тут всерьез, вам нужно быть там.

> **[посмотреть whitelist.txt](./whitelist.txt)**
>
> **[посмотреть ipwhitelist.txt](./ipwhitelist.txt)**
>
> **[посмотреть cidrwhitelist.txt](./cidrwhitelist.txt)**

---
