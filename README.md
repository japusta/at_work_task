1.Опишите, что такое инкапсуляция, наследование и полиморфизм. Приведите примеры их
применения в коде. 
```
Инкапсуляция — это способ скрыть внутренние детали объекта и предоставить только необходимые для использования его методы и свойства.
 Это помогает защитить данные от нежелательных изменений и делает код более понятным и управляемым.

Наследование — позволяет создавать новый класс на основе уже существующего, унаследовав его свойства и методы.
 Это помогает избежать повторения кода и организовать классы в иерархию.

Полиморфизм — позволяет использовать объекты разных классов единообразно, если они имеют одинаковые методы.
 Это значит, что один и тот же метод может вести себя по-разному в разных классах.

Инкапсуляция защищает внутренние данные объекта и предоставляет методы для взаимодействия с ними.
Наследование позволяет создавать новые классы на основе существующих, наследуя их свойства и методы.
Полиморфизм позволяет использовать объекты разных классов через общий интерфейс, вызывая их методы по-разному.
```
Инкапсуляция:
```
#include <iostream>
#include <string>
class Phone {
private:
    std::string model;
    int batteryLevel; // Приватный атрибут
public:
    Phone(const std::string& m, int battery) : model(m), batteryLevel(battery) {}
    // Метод для получения модели
    std::string getModel() const {
        return model;
    }
    // Метод для получения уровня батареи
    int getBatteryLevel() const {
        return batteryLevel;
    }
    void charge(int amount) {
        batteryLevel += amount;
        if (batteryLevel > 100) {
            batteryLevel = 100;
        }
    }
};
int main() {
    Phone myPhone("iPhone", 50);
    std::cout << "Model: " << myPhone.getModel() << std::endl;                 // Вывод: iPhone
    std::cout << "Battery Level: " << myPhone.getBatteryLevel() << "%" << std::endl; // Вывод: 50%
    myPhone.charge(30);
    std::cout << "Battery Level after charging: " << myPhone.getBatteryLevel() << "%" << std::endl; // Вывод: 80%
    return 0;
}
```
Наследование.
```
#include <iostream>
#include <string>

// Базовый класс 
class Phone {
private:
    std::string model;
    int batteryLevel;
public:
    Phone(const std::string& m, int battery) : model(m), batteryLevel(battery) {}
    std::string getModel() const {
        return model;
    }
    int getBatteryLevel() const {
        return batteryLevel;
    }
    void charge(int amount) {
        batteryLevel += amount;
        if (batteryLevel > 100) {
            batteryLevel = 100;
        }
    }
    // Виртуальный метод для демонстрации полиморфизма
    virtual void showInfo() const {
        std::cout << "Model: " << model << ", Battery Level: " << batteryLevel << "%" << std::endl;
    }
};
// Производный класс 
class Smartphone : public Phone {
private:
    int cameraMegapixels;
public:
    Smartphone(const std::string& m, int battery, int camera)
        : Phone(m, battery), cameraMegapixels(camera) {}
    int getCameraMegapixels() const {
        return cameraMegapixels;
    }
    void showInfo() const override {
        Phone::showInfo();
        std::cout << "Camera: " << cameraMegapixels << " MP" << std::endl;
    }
    void takePhoto() const {
        std::cout << "Photo taken with a " << cameraMegapixels << " MP camera." << std::endl;
    }
};
int main() {
    Phone regularPhone("Nokia", 40);
    Smartphone mySmartphone("Samsung Galaxy", 70, 12);
    regularPhone.showInfo();   // Вывод: Model: Nokia, Battery Level: 40%
    mySmartphone.showInfo();   //Вывод:  Model: Samsung Galaxy, Battery Level: 70%, Camera: 12 MP
    mySmartphone.charge(20);
    std::cout << "Battery Level after charging: " << mySmartphone.getBatteryLevel() << "%" << std::endl;    // Вывод: Battery Level after charging: 90%
    mySmartphone.takePhoto();  // Вывод: Photo taken with a 12 MP camera.
    return 0;
}
```
Полиморфизм.
```
#include <iostream>
#include <string>
#include <vector>
#include <memory>
class Device { // Базовый абстрактный класс Device
public:
    virtual void showInfo() const = 0; // Чисто виртуальный метод
    virtual ~Device() {}
};
class Phone : public Device { // Класс Phone
private:
    std::string model;
    int batteryLevel;
public:
    Phone(const std::string& m, int battery) : model(m), batteryLevel(battery) {}
    void showInfo() const override {
        std::cout << "Phone - Model: " << model << ", Battery Level: " << batteryLevel << "%" << std::endl;
    }
};
class Smartphone : public Device { // Класс Smartphone
private:
    std::string model;
    int batteryLevel;
    int cameraMegapixels;
public:
    Smartphone(const std::string& m, int battery, int camera) : model(m), batteryLevel(battery), cameraMegapixels(camera) {}
    void showInfo() const override {
        std::cout << "Smartphone - Model: " << model
                  << ", Battery Level: " << batteryLevel
                  << "%, Camera: " << cameraMegapixels << " MP" << std::endl;
    }
};
class PlasticPhone : public Device {  // Класс PlasticPhone
private:
    std::string model;
    int batteryLevel;
public:
    PlasticPhone(const std::string& m, int battery) : model(m), batteryLevel(battery) {}
    void showInfo() const override {
        std::cout << "Plastic Phone - Model: " << model
                  << ", Battery Level: " << batteryLevel << "%" << std::endl;
    }
};
void displayDeviceInfo(const Device& device) {    // Функция для демонстрации полиморфизма
    device.showInfo();
}
int main() {
    Phone phone("Nokia", 40);
    Smartphone smartphone("Samsung Galaxy", 70, 12);
    PlasticPhone plasticPhone("PlasticPhone", 30);
    std::vector<std::shared_ptr<Device>> devices;
    devices.push_back(std::make_shared<Phone>(phone));
    devices.push_back(std::make_shared<Smartphone>(smartphone));
    devices.push_back(std::make_shared<PlasticPhone>(plasticPhone));
    for (const auto& device : devices) {
        device->showInfo();
    }
    // Вывод:
    // Phone - Model: Nokia, Battery Level: 40%
    // Smartphone - Model: Samsung Galaxy, Battery Level: 70%, Camera: 12 MP
    // Plastic Phone - Model: PlasticPhone, Battery Level: 30%
    return 0;
}
```



2. Опишите, как бы вы восстановили предыдущую версию кода в git после
внесения ошибки
```
Восстановление предыдущей версии кода в Git можно выполнить несколькими способами в зависимости от ситуации.
 git revert для безопасной отмены изменений,
особенно если изменения уже отправлены в удалённый репозиторий.
git reset подходит для локальных исправлений.

Если изменения уже отправлены на сервер и над проектом работают несколько человек, я бы использовал git revert.
Эта команда создаёт новый коммит, который отменяет изменения, внесённые ошибочным коммитом. Это безопасно и не нарушает историю проекта.
git revert <hash_коммита>
Например, если хеш коммита с ошибкой abc1234, команда будет выглядеть так:
git revert abc1234

Если изменения ещё не отправлены в удалённый репозиторий или я уверен, что никто другой не зависит от этих коммитов,
я могу использовать git reset. Есть два варианта:
1. (--soft) Эта команда перемещает указатель ветки на выбранный коммит, но сохраняет изменения в рабочей директории и индексе.
 Это полезно, если я хочу внести правки и сделать новый коммит. git reset --soft <hash_коммита>

2. (--hard) Эта команда полностью откатывает состояние проекта до выбранного коммита, удаляя все последующие изменения.
 незакоммиченные изменения будут потеряны. git reset --hard <hash_коммита>
```

3. Напишите функцию на любом языке, которая проверяет, является ли строка
палиндромом.
```
function isPalindrome(str) {
    // Удаляем все символы, кроме букв (включая кириллицу) и цифр, приводим к нижнему регистру
    const cleaned = str.replace(/[^A-Za-zА-Яа-я0-9]/g, '').toLowerCase();
    const reversed = cleaned.split('').reverse().join('');
    return cleaned === reversed;
}
// Примеры использования
console.log(isPalindrome("Кони, топот, инок")); // Output: true
console.log(isPalindrome("Hello World"));                 // Output: false
```

4. Кратко расскажите о вашем опыте в IT и поделитесь ссылкой на проекты на
GitHub, если они у вас есть.
```
учусь на пятом курсе в СПБПУ по специальности компьютерная безопасность, на 3 курсе проходил стажировку в LG,
 занимался разработкой web-приложения для smartTV телевизоров LG,
на 4 курсе в институте занимался разработкой в рамках курсовых работ веб-интерфейсом для LFS (Linux From Scratch),
 а также по дисциплине "безопасность интернет-приложений" занимался разработкой веб-приложения "культурный навигатор",
который позволял пользователям делиться впечатлениями о посещенных местах, и строить раличные виды маршрутов между местами по карте.
 Разработку интерфейсов вёл с помощью React, в качестве БД использовал mongoDB,
также размещал данное прилоежние на хостинге, настраивал Nginx сервер для приложения,
 тестировал приложение на безопасность (сканеры ZAP, Acunetix) и устранял обнаруженные уязвимости,
также тестировал приложение на подверженность атакам ARP spoofing и MitM.
 На гитхабе находится 14 репозиториев с проектами,
в свободное от учебы время стараюсь как можно чаще заниматься проектами для портфолио,
 также некоторые проекты имеют как frontend, так и backend сторону.

Ссылка на github: https://github.com/japusta?tab=repositories
```
