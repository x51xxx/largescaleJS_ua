<!-- ### Що з себе представляє «великий» JavaScript додаток? -->

Перед тим як я почну, давайте спробуємо визначити, що саме ми маємо
на увазі, коли говоримо про великі JavaScript-додатках. Це питання
я вважаю свого роду викликом досвідченим розробникам, і відповіді на нього,
відповідно, виходять дуже суб'єктивними.

Заради експерименту я запропонував кільком середньостатистичним розробникам
дати власне визначення цьому терміну. Один з розробників сказав, що
мова йде про «JavaScript-додатках, що складаються з більш ніж 100 000
рядків коду», коли інший визначив, що велике додаток «містить більше
ніж 1 МБ JavaScript коду». Я засмутив сміливців - обидва цих варіанти
далекі від істини. Кількість коду не завжди корелюється зі складністю програми.
100 000 рядків легко можуть виявитися самим нічим не примітним простим кодом.

Я не знаю, чи підходить моє власне визначення до будь-якої нагоди, але я вірю,
що воно знаходиться ближче всього до того, що дійсно представляє із себе
велике JavaScript-додаток.

{:class="message"}
Я думаю, що великі JavaScript-додатки вирішують нетривіальні завдання,
а підтримка таких програм вимагає від розробника серйозних зусиль.
При цьому, велика частина роботи по маніпуляції даними та їх відображення лягає
на браузер.
