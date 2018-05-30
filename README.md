#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cmath>
#include <vector>
#include<string>
#include <algorithm>
#include <utility>

using namespace std;

class Hero {
protected:
	string name;
	int health;
	int mana;
	int speed_attack;
public:
	Hero(string a, int b, int c, int d) :
		name(a), health(b), mana(c), speed_attack(d)
	{}
	string print_name() {
		return this->name;
	}
	int print_health() {
		return this->health;
	}
	int print_mana() {
		return this->mana;
	}
	int print_speed_atack() {
		return this->speed_attack;
	}
	virtual int  fight() = 0;
	virtual int get_damage(int a) = 0;
};

class Tank : public Hero {

private:
	int strength;
	int base_damage;

public:
	Tank(string a, int b, int c, int d, int e, int f)
		: Hero(a, b, c, d), strength(e), base_damage(f)
	{}
	int print_strength() {
		return this->strength;
	}
	int fight() {
		return this->base_damage*this->strength;
	}
	int get_damage(int a) {
		this->health = this->health*this->strength;
		this->health = this->health - a;
		return this->health;
	};

};

class Wizard : public Hero {

private:
	int intelligence;
	int magic_resist;

public:
	Wizard(string a, int b, int c, int d, int e, int f)
		:Hero(a, b, c, d), intelligence(e), magic_resist(f)
	{}
	int print_intelligence() {
		return this->intelligence;
	}
	int fight() {
		int base_damage = 20;
		int damage = base_damage*this->intelligence;;
		return damage;
	}
	int get_damage(int a) {
		int base_heal = 10;
		this->health = this->health + base_heal*this->intelligence;
		this->health = this->health - (1 - (this->magic_resist / 100)) * a;
		return this->health;
	};
};

class Dodger : public Hero {

private:
	int agility;

public:
	Dodger(string a, int b, int c, int d, int e)
		:Hero(a, b, c, d), agility(e)
	{}
	int print_agility() {
		return this->agility;
	}
	int fight() {
		this->speed_attack = this->speed_attack*this->agility;
		return this->speed_attack;
	}
	int get_damage(int a) {
		int k = rand() % 2;
		if (k == 0) {
			return this->health;
		}
		else {
			return this->health - a;
		}
	};
};

void meeting(Hero* A, Hero* B) {
	int HEALTH1 = A->get_damage(B->fight());
	int HEALTH2 = B->get_damage(A->fight());
	if (HEALTH1 >= 0 && HEALTH2 >= 0 || HEALTH1 <= 0 && HEALTH2 <= 0) {
		cout << "DRAW" << endl;
	}
	else if (HEALTH1 > 0 && HEALTH2 <= 0) {
		cout << A->print_name() << " " << "WIN" << endl;
	}
	else {
		cout << B->print_name() << " " << "WIN" << endl;
	}
}

int main() {
	Wizard a("Slayer", 100, 200, 50, 40, 10);
	Tank b("Axe", 200, 100, 30, 50, 20);
	Dodger c("Hunter", 1000, 100, 150, 60);
	meeting(&a, &c);
	meeting(&a, &b);
	meeting(&b, &c);
	return 0;
}
