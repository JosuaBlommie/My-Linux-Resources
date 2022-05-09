Classes are defined with the "class" keyword, and should always start with a capital letter, ex:

    class User:
        pass
  
Create an **object** or an **instance** of a class, by calling the class name with parentheses:

    user1 = User()

There is no limit to the number of objects that can be made from a class.

Data attached to an object is called a **field**, do not capitilise the name of fields, use underscores to seperate words:

    user1.first_name = "Blah"
    user1.last_naem = "Bleh"

Different objects from the same class can have varying amounts of unique fields assigned to it. If a field is called for an object that does not have that field assigned to it, an AttributeError is given

To access the data/field in the object of the class User, type the same way it was assigned:

    print(user1.first_name)

A separate variable can have the exact same name as the field assigned to an object, but will contain a different value if called as part of the object field. Ex:

    first_name = "Bloo" #will contain "Bloo"
    user.first_name #will contain "Blah" as assigned above

Classes have the following additional features (over dicts for example):
    - Methods - a function inside a class is called a method
    - Initialization - enabled through the init method
    - Help text - enabled through the docstring

The init method is a feature that can be added to python classes (init methods in other languages are sometimes called "constructors")

    class User:
        def __init__(self,full_name, birthday):

The init method is called everytime a new instance/object of the class is created. The first argument in the method is "self", which is a reference to the new object that has been created

First thing to do in init method is store argument values to a field in the newly created object:

    class User:
        def __init__(self,full_name, birthday):
            self.name = full_name
            self.birthday = birthday 
            
            # Exract first and last names
            name.pieces = full_name.split(" ")
            self.first_name = name_pieces[0]    # This defines a new field attributed to the object that self refers to
            last_name = name_pieces[-1]         # This is a variable, not a field, and the value of this variable only exists until the end of the method init
            
"birthday" is the value of the argument passed throught the method, "self.birthday" is the field that stores the value. To create user object using the init mathod, and call the field values of that object:

    user = User("Bloo Blah", "20200301")
    print(user.name)
    print(user.birthday)
    print(user.first_name)
    print(user.last_name)   # All of the above will run fine, but this will give "AttributeError" because the field was not attached to the object using self
    
Can further improve the class by creating help text, help text is define by a docstring, which is written inside tripple quotes, right after the first line of the class name:

    class User:
        """This is the help text"""
        def __init__(self,full_name, birthday):
        
The help function of a class can be called, which then displays the help text and methods define in the class, and the arguments expected in the init method

    help(User)
    
Additional methods can be added within classes, like the init method, the first argument is "self". This method oes not require additional arguments, because all information is captured by fields in the init method, and available by calling field name through self

    class User:
        def __init__(self,full_name, birthday):
            self.name = full_name
            self.birthday = birthday

        def age(self):
            """Returns the age of user in years"""
            year = int(self.birthday[0:4])
            ...etc
            return int(age_in_years)
            
Can test by creating new user object from class. When calling a method within a class, the self keyword is only used when writing the method:

    new_user = User("Bloo Blah", "20200301")
    print(new_user.age())   # Will print the int returned byt he age() method. 
    
    
        
    

