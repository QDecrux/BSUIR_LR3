# LR3_repo

Перед вами репозиторий для лабораторных работ. Тут будут размещаются условия лабораторных работ, вспомогательные файлы и документация.

# Общая информация
## Ветки
Главной является ветка `master`, ее может редактировать только преподаватель. В ней располагается данный файл, условия работ и дополнительные материалы.
Для вашей работы, вам необходимо создать свою "официальную" ветку (*branch*) с названием, соответствующим вашей фамилии и номеру по журналу. Например, `13_marchenko`.
Впрочем, вам разрешено создавать произвольное число веток, лишь бы вы оставляли их локальными (то есть **не заливали их сюда**). 

*Относитесь к репозиторию с уважением, иначе он быстро превратится в мусорку*
## Условия задачек
Теперь у вас есть возможность получать условия задачек по мере их поступления. Как только в ветке `master` появляется новая папка, смело подтягивайте ее (`pull`), в ней будет `readme.md` файл с условием, а также дополнительные материалы, необходимые для выполнения. 
## Отправка решения и проверка
Все просто: готовое решение должно постепенно вливаться в вашу "официальную" ветку. Периодически синхнонизируйте ее с репозиторием с помощью команды `push`.

**Проверка** работы будет производиться только в университете только по вашему желанию. Если у вас возникают вопросы, вы можете легко ссылаться на код в репозитории.
## О честности и безопасности
Гит *не предоставляет* простых способов контроля за чтением веток. Это значит, что любой ваш одногруппник может влезть в ваш код, списать или даже накосячить! Если вкратце, это ваши проблемы! На деле все не так страшно, ведь гит сохраняет вообще все верисии вашего кода. Так что кто-то должен просто космически накосячить, чтобы ваша работа пропала бесследно. Кроме того, гит предоставляет весьма простые средства, позволяющие установить, кто именно редактировал код. 
Чтобы минимизировать риски, просто следуйте следующим правилам:

- Подтягивайте (`pull`) изменения только **своей** ветки или ветки **master**.
- Заливайте (`push`) в репозиторий только **свою официальную ветку** по мере необходимости: задание готово, нужно чего-то спросить, нужно дать кому-то скатать...
- Перед тем как делать коммит, убедитесь, что вы находитесь в своей ветке: `git branch`
- Почаще делайте эти самые коммиты.

*Если все будут следовать этим указаниям, серьезных проблем возникнуть не должно*

## Как быстро подучить git
- [Как пользоваться git](https://githowto.com/ru)

# Структура проекта
Ветка - это версия (копия) проекта. В главное ветке `master` будет лежать данный `readme.md` файл и также папки с номерами работ. В этих папках будут лежать свои readme файлы с описанием заданий, а также дополнительные материалы. 

**Ваша ветка** будет копией ветки `master`, но с решениями. То есть, вы можете создавать проекты прямо в папках с номерами работ. Так что в итоге у всех в ветке будет `readme.md` файл и куча папок, по одной на каждое задание.

# Вспомогательный материал

## Ссылки

- [Стратегии управления ресурсами](https://habr.com/post/425837/)
- [Move-семантика](https://habr.com/post/226229/)

## Код с занятия


```cpp
#include <iostream> 
#include <vector> 
#include <string> 
#include <initializer_list> 

using namespace std;

class Vector
{
public:
	Vector()
	{}
	Vector(const Vector& vec) = delete;
	Vector& operator=(const Vector& vec) = delete;
	Vector(Vector&& vec) :
		ms(vec.ms), size(vec.size)
	{
		vec.ms = nullptr;
		vec.size = 0;
	}
	Vector& operator=(Vector&& vec)
	{
		if (ms != nullptr) delete[] ms;

		ms = vec.ms;
		size = vec.size;

		vec.ms = nullptr;
		vec.size = 0;

		return *this;
	}

	Vector(const initializer_list<int>& list)
	{
		ms = new int[list.size()];
		size = 0;
		for (const auto& a : list)
		{
			ms[size] = a;
			++size;
		}
	}

	~Vector()
	{
		if (ms != nullptr) delete[] ms;
	}

	void push_back(const int n)
	{
		int* p = new int[size + 1];
		for (int i = 0; i < size; ++i)
		{
			p[i] = ms[i];
		}

		if (ms != nullptr) delete[] ms;
		ms = p;
		ms[size] = n;
		++size;
	}
	
	const int* begin() const
	{
		return ms;
	}
	const int* end() const
	{
		return ms + size;;
	}

	void pop_back()
	{
		if (size == 0) throw logic_error("Pop from empty vector");
		int* p = new int[size - 1];
		for (int i = 0; i < size - 1; ++i)
		{
			p[i] = ms[i];
		}

		if (ms != nullptr) delete[] ms;
		ms = p;
		--size;
	}
	size_t Size() const
	{
		return size;
	}

	int& operator[](const int index)
	{
		return ms[index];
	}
	int operator[](const int index) const
	{
		return ms[index];
	}

	bool empty() const
	{
		return size == 0;
	}

	explicit operator bool()
	{
		return !empty();
	}

private:
	size_t size = 0;
	int* ms = nullptr;
};

ostream& operator<<(ostream& stream, const Vector& vec)
{
	for (auto a : vec)
	{
		stream << a << " ";
	}
	return stream;
}


void swap(Vector& a, Vector& b) noexcept
{
	Vector temp(move(a));
	a = move(b);
	b = move(temp);
}

Vector Generate()
{
	Vector v;
	v.push_back(42);
	v.push_back(43);
	v.push_back(44);
	return v;
}

int main()
{
	Vector v = { 9, 90, 88, 77, 44, 2, 1 };
	cout << v;

	cout << "\nThe end!" << endl;
	system("pause > NUL");
	return 0;
}
```