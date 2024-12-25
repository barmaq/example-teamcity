# Домашнее задание к занятию 11 «Teamcity»

## Подготовка к выполнению

создал виртуалки пользуясь видео.
за рабочий плейбук отдельное спасибо! понимаю, что это полезный опыт, но исправлять несоответсвия версий, интерпритаторов, репозиториев ушедших в архив и т д, отнимает много времени, которого обычно всегда мало. ну и поднабрался в этом опыта уже :)

## Основная часть

1. Создайте новый проект в teamcity на основе fork.  
создал проект netology  
2. Сделайте autodetect конфигурации.  
обнаружил конфигурацию maven в репозитории  
![maven](./images/2.png)  
3. Сохраните необходимые шаги, запустите первую сборку master.  
успешно  
![run](./images/3.png)  
4. Поменяйте условия сборки: если сборка по ветке `master`, то должен происходит `mvn clean deploy`, иначе `mvn clean test`.  
добавил еще одним шагом. может неправильно и есть вариант с if else ?  
![step2](./images/4.png)  
5. Для deploy будет необходимо загрузить [settings.xml](./teamcity/settings.xml) в набор конфигураций maven у teamcity, предварительно записав туда креды для подключения к nexus.  
добавил  
![settings](./images/5.png)   
6. В pom.xml необходимо поменять ссылки на репозиторий и nexus.  
сделал в самом начале.   
7. Запустите сборку по master, убедитесь, что всё прошло успешно и артефакт появился в nexus.  
![artefact](./images/7.png)   
8. Мигрируйте `build configuration` в репозиторий.
настроил синхронизацию. при изменении билд будет пересобираться ( был приведен как один из вариантов миграции, если надо было разово - укажите ).   
![sync](./images/8.png)  
9. Создайте отдельную ветку `feature/add_reply` в репозитории.  
готово  
![branch](./images/9.png)  
10. Напишите новый метод для класса Welcomer: метод должен возвращать произвольную реплику, содержащую слово `hunter`.  
дополнил :

example-teamcity/src/main/java/plaindoll/HelloPlayer.java 

---  
```
package plaindoll;

public class HelloPlayer{
	public static void main(String[] args) {
		Welcomer welcomer = new Welcomer();
		System.out.println(welcomer.sayWelcome());
		System.out.println(welcomer.sayFarewell());
		System.out.println(welcomer.sayHunter());
	}
}
```

src/main/java/plaindoll/Welcomer.java  

--- 
```
package plaindoll;

public class Welcomer{
	public String sayWelcome() {
		return "Welcome home, good hunter. What is it your desire?";
	}
	public String sayFarewell() {
		return "Farewell, good hunter. May you find your worth in waking world.";
	}
	public String sayNeedGold(){
		return "Not enough gold";
	}
	public String saySome(){
		return "something in the way";
	}
	public String sayHunter(){
		return "See You Space Hunter";
	}

}
```

11. Дополните тест для нового метода на поиск слова `hunter` в новой реплике.  
добавляем в тест
```
	@Test
	public void welcomerSaysSpaceHunter(){
		assertThat(welcomer.sayHunter(), containsString("something"));
	}

```

12. Сделайте push всех изменений в новую ветку репозитория.  
Done!  
13. Убедитесь, что сборка самостоятельно запустилась, тесты прошли успешно.  
Ощибка в предыдущей сборки изза того что не менял версию в nexus. скорректировал и все заработало
![test2](./images/14.png) 
14. Внесите изменения из произвольной ветки `feature/add_reply` в `master` через `Merge`.  
Done!  
15. Убедитесь, что нет собранного артефакта в сборке по ветке `master`.  
Артефакта нет  
16. Настройте конфигурацию так, чтобы она собирала `.jar` в артефакты сборки.  
то ли проглядел , то ли не было в лекции. пришлось пользоваться помощью интернета
![jar](./images/16.png) 
17. Проведите повторную сборку мастера, убедитесь, что сбора прошла успешно и артефакты собраны.
![total](./images/17.png) 
18. Проверьте, что конфигурация в репозитории содержит все настройки конфигурации из teamcity.
[teamcity](https://github.com/barmaq/example-teamcity/tree/master/.teamcity/Netology)  
19. В ответе пришлите ссылку на репозиторий.
[Repo](https://github.com/barmaq/example-teamcity) 

---
