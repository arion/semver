Семантическая версионность 1.0.0-rc.1
==============================

В мире разработки программного обеспечения, существует страшное место,
называемое "ад зависимостей". Чем больше ваша система, тем больше шанс, что
в один из дней вы попадете в эту ловушку.

В системе с большим количеством зависимостей, выпуск новых пакетов может быстро
превратиться в кошмар. Если зависимости слишком прочные, вы не можете обновить
пакет, не обновив при этом версии всех зависимых пакетов. Если зависимости
слишком свободные, у вас возникнут проблемы с распущенностью версий. 
"Ад зависимостей", это когда слишком прочные, или наоборот, слишком свободные
зависимости не дают вам легко и безопасно развивать ваш проект.

Как решение этой проблемы, я предлагаю простой набор правил, которые диктуют
как присваивать и увеличивать номера версий. Для того, что бы система
работала, для начала, вам нужно объявить открытый API. Это должен быть
простой и ясный документ. Рассмотрим версию формата X.Y.Z (Major.Minor.Patch).
При исправление ошибок не затрагивающих API увеличивается Path версия. При
добавление или изменение API с сохранением обратной совместимостью увеличивается 
Minor версия. При изменение API без сохранение обратной совместимости увеличивается 
Major версия.

Я называю эту систему "Семантическая версионность". Согласно этой схеме, номера
версий и то, как они меняются несут в себе смысл об основном коде и о том, как
он изменялся от версии к версии.


Спецификация семантической версионности
------------------------------------------

Ключевые слова "ДОЛЖЕН (MUST)", "НЕ ДОЛЖЕН (MUST NOT)", "СЛЕДУЕТ (SHOULD)", 
"НЕ СЛЕДУЕТ (SHOULD NOT)", "МОЖЕТ (MAY)", в этом документе должны 
интерпретировать в соответствии с RFC 2119.

1. Программный продукт использующий Семантическую Весионность 
ДОЛЖЕН иметь открытый API. Это API должно быть объявлено внутри кода
или в прикладной документации. API должно быть точным и исчерпывающим.

2. Номер версии ДОЛЖЕН состоял из X.Y.Z, где X, Y, и Z это
положительные числа. X это Major версия, Y это Minor версия и Z это Path
версия. Каждый элеме ДОЛЖЕН увеличиваться с шагом один.
Например: 1.9.0 -> 1.10.0 -> 1.11.0.

3. Когда Major версия увеличивается, Minor и Path версии ДОЛЖНЫ обнуляться.
Когда Minor версия увеличивается, Path версия должны обнуляться. Нарпимер:
1.1.3 -> 2.0.0 и 2.1.7 -> 2.2.0.

4. После того как версия пакета выпущена, в этот пакет НЕ ДОЛЖНО вносится
никаких изменений. Все изменения ДОЛЖНЫ выпускаться с новой версией.

5. В начале разработки Major версия равна нулю (0.y.z). В этот период, 
что нибудь может изменится в любое время. Открытое API НЕ ДОЛЖНО считаться 
стабильным.

6. Версия 1.0.0 определяет открытое API. То, как изменяется номер версии, 
зависит от этого открытого API.

7. Path версия Z (x.y.Z | x > 0) ДОЛЖНА быть увеличена только если исправления
ошибок имеют обратную совместимость с предидущими версиями. Исправление
ошибок устраняет некорректное поведение.

8. Minor версия Y (x.Y.z | x > 0) ДОЛЖНА быть увеличена если внесены исправления,
совместимые с предидущими версиями. Minor версия ДОЛЖНА быть увеличена если
какой либо элемент API помечен как "Устаревший". Minor версия МОЖЕТ быть увеличена,
при наличии существенных новых функциональных возможностей или улучшений. Minor 
версия МОЖЕТ включать изменения Path версии. Path версия должна обнуляться, 
когда изменяется Minor версия.

9. Major версия X (X.y.z | X > 0) ДОЛЖНА быть увеличена если внесены исправления,
несовместимые с предидущими версиями. Major версия МОЖЕТ включать изменения
Minor и Path версий. Path и Minor версии должны обнуляться, 
когда изменяется Major версия.

10. Предварительные версии МОГУТ быть обозначены тире и идентификатором, разделенным
точками, сразу после PATH версии. Идентивикатор ДОЛЖЕН содержать символы [0-9A-Za-z-].
Предварительная версия имеет меньший приоритет, чем нормальная версия.
Примеры: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92.

11. Версия сборки МОЖЕТ быть обозначена знаком плюс и идентификатором, разделенным
точками, сразу же после версии Path или предварительной версии. Идентивикатор 
ДОЛЖЕН содержать символы [0-9A-Za-z-]. Версия сборки имеет больший приоритет, 
чем нормальная версия.
Примеры: 1.0.0+build.1, 1.3.7+build.11.e0f985a.

12. Приоритет ДОЛЖЕН рассчитываться путем разделения на Major, Minor, Path,
предварительные и сборочные версии. Major, Minor и Path версии всегда содержат
цифры. Предварительные и сборочные версии ДОЛЖНЫ быть отсортированы путем сравнения
каждого идентификатора разделенного точкой по следующей схеме: Идентификаторы
содержащие только цифры сравниваются циферно, содержащие буквы по порядку указанному
в ASCII. Циферные идентификаторы всегда имеют меньший приоритет чем буквенные.
Примеры: 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-beta.2 < 1.0.0-beta.11 <
1.0.0-rc.1 < 1.0.0-rc.1+build.1 < 1.0.0 < 1.0.0+0.3.7 < 1.3.7+build <
1.3.7+build.2.b8f12d7 < 1.3.7+build.11.e0f985a.

Почему нужно использовать Семантическую Версионность?
----------------------------

Это не новая или революционная идея. На самом деле, вы уже наверное делали, что
то похожее. Проблема в том, что "похожее" не является достаточно хорошим. Без
соответствия какой-то формальной спецификации, номера версий по существу 
бесполезны для управления зависимостями. Давая ясные и в меру гибкие определения, 
как выше, вы облегчаете взаимодействие пользователей с вашим продуктом. 

A simple example will demonstrate how Semantic Versioning can make dependency
hell a thing of the past. Consider a library called "Firetruck." It requires a
Semantically Versioned package named "Ladder." At the time that Firetruck is
created, Ladder is at version 3.1.0. Since Firetruck uses some functionality
that was first introduced in 3.1.0, you can safely specify the Ladder
dependency as greater than or equal to 3.1.0 but less than 4.0.0. Now, when
Ladder version 3.1.1 and 3.2.0 become available, you can release them to your
package management system and know that they will be compatible with existing
dependent software.

As a responsible developer you will, of course, want to verify that any
package upgrades function as advertised. The real world is a messy place;
there's nothing we can do about that but be vigilant. What you can do is let
Semantic Versioning provide you with a sane way to release and upgrade
packages without having to roll new versions of dependent packages, saving you
time and hassle.

If all of this sounds desirable, all you need to do to start using Semantic
Versioning is to declare that you are doing so and then follow the rules. Link
to this website from your README so others know the rules and can benefit from
them.


FAQ
---

### How should I deal with revisions in the 0.y.z initial development phase?

The simplest thing to do is start your initial development release at 0.1.0
and then increment the minor version for each subsequent release.

### How do I know when to release 1.0.0?

If your software is being used in production, it should probably already be
1.0.0. If you have a stable API on which users have come to depend, you should
be 1.0.0. If you're worrying a lot about backwards compatibility, you should
probably already be 1.0.0.

### Doesn't this discourage rapid development and fast iteration?

Major version zero is all about rapid development. If you're changing the API
every day you should either still be in version 0.x.x or on a separate
development branch working on the next major version.

### If even the tiniest backwards incompatible changes to the public API require a major version bump, won't I end up at version 42.0.0 very rapidly?

This is a question of responsible development and foresight. Incompatible
changes should not be introduced lightly to software that has a lot of
dependent code. The cost that must be incurred to upgrade can be significant.
Having to bump major versions to release incompatible changes means you'll
think through the impact of your changes, and evaluate the cost/benefit ratio
involved.

### Documenting the entire public API is too much work!

It is your responsibility as a professional developer to properly document
software that is intended for use by others. Managing software complexity is a
hugely important part of keeping a project efficient, and that's hard to do if
nobody knows how to use your software, or what methods are safe to call. In
the long run, Semantic Versioning, and the insistence on a well defined public
API can keep everyone and everything running smoothly.

### What do I do if I accidentally release a backwards incompatible change as a minor version?

As soon as you realize that you've broken the Semantic Versioning spec, fix
the problem and release a new minor version that corrects the problem and
restores backwards compatibility. Remember, it is unacceptable to modify
versioned releases, even under this circumstance. If it's appropriate,
document the offending version and inform your users of the problem so that
they are aware of the offending version.

### What should I do if I update my own dependencies without changing the public API?

That would be considered compatible since it does not affect the public API.
Software that explicitly depends on the same dependencies as your package
should have their own dependency specifications and the author will notice any
conflicts. Determining whether the change is a patch level or minor level
modification depends on whether you updated your dependencies in order to fix
a bug or introduce new functionality. I would usually expect additional code
for the latter instance, in which case it's obviously a minor level increment.

### What should I do if the bug that is being fixed returns the code to being compliant with the public API (i.e. the code was incorrectly out of sync with the public API documentation)?

Use your best judgment. If you have a huge audience that will be drastically
impacted by changing the behavior back to what the public API intended, then
it may be best to perform a major version release, even though the fix could
strictly be considered a patch release. Remember, Semantic Versioning is all
about conveying meaning by how the version number changes. If these changes
are important to your users, use the version number to inform them.

### How should I handle deprecating functionality?

Deprecating existing functionality is a normal part of software development and is often required to make forward progress. When you deprecate part of your public API, you should do two things: (1) update your documentation to let users know about the change, (2) issue a new minor release with the deprecation in place. Before you completely remove the functionality in a new major release there should be at least one minor release that contains the deprecation so that users can smoothly transition to the new API.


About
-----

The Semantic Versioning specification is authored by [Tom Preston-Werner](http://tom.preston-werner.com), inventor of Gravatars and cofounder of GitHub.

If you'd like to leave feedback, please [open an issue on GitHub](https://github.com/mojombo/semver/issues).


License
-------

Creative Commons - CC BY 3.0
http://creativecommons.org/licenses/by/3.0/