class ATM:
    def __init__(self):
        # قاعدة بيانات بسيطة لتخزين بيانات المستخدمين
        self.users = {
            "123456": {"pin": "1234", "balance": 1000},
            "654321": {"pin": "4321", "balance": 500},
        }
        self.current_user = None

    def authenticate(self, user_id, pin):
        """التحقق من الرقم السري"""
        if user_id in self.users and self.users[user_id]["pin"] == pin:
            self.current_user = user_id
            return True
        return False

    def check_balance(self):
        """عرض الرصيد"""
        if self.current_user:
            return self.users[self.current_user]["balance"]
        return None

    def withdraw(self, amount):
        """سحب الأموال"""
        if self.current_user:
            if self.users[self.current_user]["balance"] >= amount:
                self.users[self.current_user]["balance"] -= amount
                return f"تم سحب {amount} بنجاح. الرصيد الحالي: {self.users[self.current_user]['balance']}"
            else:
                return "رصيدك غير كافي."
        return "الرجاء تسجيل الدخول أولاً."

    def deposit(self, amount):
        """إيداع الأموال"""
        if self.current_user:
            self.users[self.current_user]["balance"] += amount
            return f"تم إيداع {amount} بنجاح. الرصيد الحالي: {self.users[self.current_user]['balance']}"
        return "الرجاء تسجيل الدخول أولاً."

    def logout(self):
        """تسجيل الخروج"""
        self.current_user = None
        return "تم تسجيل الخروج بنجاح."


# واجهة المستخدم البسيطة
def main():
    atm = ATM()
    
    while True:
        print("\nمرحباً بكم في الصراف الآلي")
        user_id = input("أدخل رقم المستخدم: ")
        pin = input("أدخل الرقم السري: ")
        
        if atm.authenticate(user_id, pin):
            print("تم تسجيل الدخول بنجاح.")
            while True:
                print("\n1. عرض الرصيد")
                print("2. سحب الأموال")
                print("3. إيداع الأموال")
                print("4. تسجيل الخروج")
                choice = input("اختر الخيار: ")
                
                if choice == "1":
                    print(f"رصيدك الحالي هو: {atm.check_balance()}")
                elif choice == "2":
                    amount = float(input("أدخل المبلغ الذي ترغب في سحبه: "))
                    print(atm.withdraw(amount))
                elif choice == "3":
                    amount = float(input("أدخل المبلغ الذي ترغب في إيداعه: "))
                    print(atm.deposit(amount))
                elif choice == "4":
                    print(atm.logout())
                    break
                else:
                    print("اختيار غير صحيح، الرجاء المحاولة مرة أخرى.")
        else:
            print("رقم المستخدم أو الرقم السري غير صحيح. الرجاء المحاولة مرة أخرى.")

if __name__ == "__main__":
    main()
