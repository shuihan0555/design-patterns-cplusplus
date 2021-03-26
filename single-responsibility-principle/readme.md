# 1 Introdution
In this article, we will see an example of the __S__ ingle __R__ esponsibility __P__ rinciple in Cplusplus along with its benefits & generic guideline:

* [x] __S__ RP - Single Responsibility Principle 
* [ ] __O__ CP - Open/Closed Principle 
* [ ] __L__ SP - Liskov Substitution Principle 
* [ ] __I__ Sp - Interface Segregation Principle 
* [ ] __D__ IP - Dependency Inversion Principle  

# 2. Single Responsibility Principle
__A class should have only one reason to change__. 

# 3 Motivation: Violating the Single Responsibility Principle (Bad example)

```cpp
class Journal 
{
	string m_title;
	vector<string> m_entries;

	public:
		explicit Journal(const string& title) : m_title(title) {
		}

		void add_entries(const string& entry) {
			static uint32_t count = 1;
			m_entries.push_back(to_string(count++) + ": " + entry);
		}

		auto get_entries() const {
			return m_entries;
		}

		void save(const string& filename) {
			ofstream ofs(filename);
			for (auto& s : m_entries) {
				ofs << s <<endl;
			}
		}
};

int main(int ac, char* av[])
{
	Journal journal{"Dear XYZ"};
	journal.add_entries("I ate a bug");
	journal.add_entries("I cried today");
	journal.save("diary.txt");

	return 0;
}
```

+ Above cplusplus example, seems fine as long as you have a single domain object. But this is not usually the case in a real-word application
+ As we start adding domain objects like __Book, File__, etc. you have to implement save method for everyone separately which is not the actual problem.
+ In this case, you have to go through every domain object implementation and need to change code all over which is not good.
+ Here, we have violate the Single Responsibility Principle by providing Journal class two reason to change.
	+ Things related to Journal
	+ Saving the Journal
+ Moreover, code will also become repetitive, bloated and hard to maintain 

# 4 Solution: Single Responsibility Principle Example in cplusplus 

```cpp
class Journal 
{
	string m_title;
	vector<string> m_entries;

	public:
		explicit Journal(const string& title) : m_title(title) {
		}

		void add_entries(const string& entry) {
			static uint32_t count = 1;
			m_entries.push_back(to_string(count++) + ": " + entry);
		}

		auto get_entries() const {
			return m_entries;
		}

		//void save(const string& filename) {
		//	ofstream ofs(filename);
		//	for (auto& s : m_entries) {
		//		ofs << s <<endl;
		//	}
		//}
};

struct SavingManager 
{
	static void save(const Journal& j, const string& filename) {
		ofstream ofs(filename);
		for (auto& s: j.get_entries()) {
			ofs << s << endl;
		}
	}
};

int main(int ac, char* av[])
{
	Journal journal{"Dear XYZ"};
	journal.add_entries("I ate a bug");
	journal.add_entries("I cried today");
	//journal.save("diary.txt");
	SavingManager::save(joural, "diary.txt");

	return 0;
}
```

