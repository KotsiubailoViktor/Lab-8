class Employee {
  constructor(firstName, lastName, baseSalary, experience) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.baseSalary = baseSalary;
    this.experience = experience;
  }

  getSalary() {
    let salary = this.baseSalary;
    if (this.experience > 5) {
      salary = this.baseSalary * 1.2 + 500;
    } else if (this.experience > 2) {
      salary += 200;
    }
    return salary;
  }
}

class Developer extends Employee {
  constructor(firstName, lastName, baseSalary, experience) {
    super(firstName, lastName, baseSalary, experience);
  }
}

class Designer extends Employee {
  constructor(firstName, lastName, baseSalary, experience, effCoeff) {
    super(firstName, lastName, baseSalary, experience);
    this.effCoeff = effCoeff;
  }

  getSalary() {
    return super.getSalary() * this.effCoeff;
  }
}

class Manager extends Employee {
  constructor(firstName, lastName, baseSalary, experience, team = []) {
    super(firstName, lastName, baseSalary, experience);
    this.team = team;
  }

  getSalary() {
    let salary = super.getSalary();
    if (this.team.length > 10) {
      salary += 300;
    } else if (this.team.length > 5) {
      salary += 200;
    }

    const developers = this.team.filter(member => member instanceof Developer).length;
    if (developers > this.team.length / 2) {
      salary *= 1.1;
    }

    return salary;
  }
}

class Department {
  constructor(managers = []) {
    this.managers = managers;
  }

  giveSalary() {
    this.managers.forEach(manager => {
      console.log(`${manager.firstName} ${manager.lastName} отримав ${manager.getSalary().toFixed(2)} шекєлей`);
      manager.team.forEach(member => {
        console.log(`${member.firstName} ${member.lastName} отримав ${member.getSalary().toFixed(2)} шекєлей`);
      });
    });
  }
}

// Приклад використання
const dev1 = new Developer('John', 'Doe', 3000, 3);
const dev2 = new Developer('Jane', 'Smith', 3200, 6);
const designer1 = new Designer('Alice', 'Johnson', 3500, 4, 0.9);
const manager1 = new Manager('Bob', 'Brown', 5000, 7, [dev1, dev2, designer1]);

const department = new Department([manager1]);
department.giveSalary();
