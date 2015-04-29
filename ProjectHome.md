## **What is PojoXML?** ##
PojoXML is a library for generating pretty xml file from POJO object and xml back to POJO.

## **PojoXML** ##
PojoXML library provides flexible way to convert  POJO to xml file and convert xml file back to POJO object. Xml generation is based on the private fields declared in the java bean object

## **How it creates xml file?** ##
PojoXML reads the pojo object instance variable one by one and invokes its getter method and obtain the value using reflection API and generates xml tags and values in the order of variable declaration in the given pojo class. Tag names default it will take as variable name. You can also assign alias name for the each and every variable and classes. So that you can generates customized xml.

```
 POJO Class
  public class Employee{        

   private String name;
   private int age;
 
   public String getName(){
     return name;
   }
   public int getAge(){
   return age;
   }
   ..............
   ..............
   ..............
} 
Generated xml file

   <?xml version="1.0" encoding="UTF-8"?>
   <Employee>
      <name>test</name>
      <age>22</age>
   </Employee>

```

## **Supported types** ##
All primitives and wrapper types,String,BigDecimal,BigInteger.
All List and Set implementations. And also supports generation of xml from   array of values.

## **How it creates POJO back from xml file?** ##
PojoXML parse the xml file and crate the instance of the given pojo Class. And set the respective values to the instance field by invoking the setter methods. PojoXML default supports dom parser default with jdk. Currently it supports only DOM parser.

## **How to Install PojoXML** ##

Download PojoXML[PojoXML](http://pojoxml.googlecode.com/files/pojoxml-1.0-bin.zip) file and set class path to pojoxml-1.0.jar file.

## **Main Classes and methods in PojoXML library** ##

Following are the main classes and methods for generating xml file and creating pojo object back from xml file
```
//PojoXml is interface. You will be using in your program to interact with library processors
org.pojoxml.core.PojoXml

//PojoXmlFactory class is used for obtaining PojoXml implementation class
org.pojoxml.core.PojoXmlFactory

Obtaining PojoXml implementation
 PojoXml pojoXml = PojoXMLFactory.createPojoXml();
```
Following are the main methods used for processing pojo and xml files.
```
public String getXml(Object object)
  This method is used for getting xml string from pojo object.
  Eg:-  String xmlContent = pojoXml.getXml(empObj);
```

```
public void saveXml(Object object,String filename)

   This method is used for generates xml file and save xml file in specific location 
Eg:- pojoXml.saveXml(empObj,”C:\\employee.xml”);
 
```

```
public void getPojo(String xmlInput,Class clas);\

   This method is used for create a pojo object from an xml string.
                                        //Casting is require  
Eg:-  Employee employee = (Employee) pojoXml.getPojo(xmlString,Employee.class);
```

```
public void getPojoFromFile (String fileName,Class clas);\

   This method is used for create a pojo object from an xml file.
                                        //Casting is require  
Eg:-  Employee employee = (Employee) pojoXml.getPojoFrormFile(fullPathNamen,Employee.class);
```

## **How PojoXML gives supports to collection framework** ##
PojoXML gives supports to all java.util.List and java.util.Set implementations. Library iterates through the collection objects defined in the pojo object and generates xml.

```
Pojo Class

public class Employee{

	private String name;
	private int age;
            List addressList;

	public String getName(){
		return name;
	}
   ..............
   ..............
   ..............
}
 Generated Xml file

<?xml version="1.0" encoding="UTF-8"?>
<Employee>
   <name>test</name>
   <age>22</age>
   <addressList>
      <Address>
         <address1>Sagar</address1>
         <address2>Nerul</address2>
         <city>Mumbai</city>
         <zip>1253</zip>
      </Address>
      <Address>
      ...........
      ...........
      ...........
      </Address>
   </addressList>
</Employee>
```

Generating pojo object back from above xml is slight different if the pojo contains collection object.
Prior to java 5, there is no way to specify what is the type of data, collection classes holds. So for supporting collection object in 1.4 and below, we handled creation of collection objects POJO in a special way.

If you don’t specify the collection type class it will throws an exception
org.pojoxml.core.PojoXmlException

```
PojoXml pojoXml = PojoXmlFactory.createPojoXml();
pojoXml.addCollectionClass(“Address”,Address.class);
Employee employee = (Employee.class) pojoXml.getPojo(employeeXml,Employee.class);
```

## **Support for arrays** ##
PojoXML supports one dimension array of primitives, wrappers, String, BigDecimal, BigInteger and user defined objects

## **CDATA (Unparsed) Character Data** ##
If your String contains the sensitive data that should not be parsed by the XML parser you can enable CDATA section. So that if any variable with type String PojoXML automatically adds a CDATA section to it.

```
Pojo Class

public class Employee{

	private String name;
	private int age;
 
	public String getName(){
		return name;
	}
	.............
        ............
        ..............
}
 Generated Xml file

<?xml version="1.0" encoding="UTF-8"?>
<Employee>
   <name><![CDATA[Mike]]></name>
   <age>22</age>
</Employee>
```
## **Adding Alias Names** ##
You can add alias name for the variables Name and class Names, So that Xml tag names will generates in a customized manner.
```


		PojoXml pojoXml = PojoXmlFactory.createPojoXml();
		;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
		;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
                        //Type of class, alias Name
		pojoXml.addClassAlias(Employee.class, "EmployeeInfo");
		//Type of class, field name, alias name
                       pojoXml.addFieldAlias(Employee.class, "name", "employeeName");

                        pojoXml.saveXml(employee);

```
```
Pojo Class

public class Employee{

	private String name;
	private int age;
 
	public String getName(){
		return name;
	}
	………………….
	;;;;;;;;;;;;;;;;;;;;;;;;;;;;
}

Generated xml file
<?xml version="1.0" encoding="UTF-8"?>
< EmployeeInfo >
   < employeeName >test</ employeeName >
   <age>22</age>
</ EmployeeInfo >
```
You must provide same alias configuration,while parsing xml back to POJO.
otherwise pojoxml cannot identify the mapping fields


## **Common Exceptions** ##
If a pojo contains collection object, and you are mapping xml file with that pojo object without specifying the collection type class
You will get following exception

See the code snippet collection class type is commented.
```

PojoXml pojoXml = PojoXmlFactory.createPojoXml();
		//specify type of the object contains in collection class
 		//pojoXml.addCollectionClass("Address",Address.class);
 		Employee employee = (Employee)pojoXml.getPojoFromFile("C:\\employee.xml",Employee.class);
```

```
Exception in thread "main" org.pojoxml.core.PojoXmlException: Class for Address is not specified## 
Specify it using PojoXmlObj.addCollectionClass("Address",Address.class) method
	at org.pojoxml.core.processor.xmltopojo.dom.XmlToCollectionHandler.readChildren(Unknown Source)
	at org.pojoxml.core.processor.xmltopojo.dom.XmlToCollectionHandler.processCollection(Unknown Source)
	at org.pojoxml.core.processor.xmltopojo.dom.XmlToObjectProcessor.processElementNodeContinue(Unknown Source)
	at org.pojoxml.core.processor.xmltopojo.dom.XmlToObjectProcessor.readChildren(Unknown Source)
	at org.pojoxml.core.processor.xmltopojo.dom.XmlToObjectProcessor.<init>(Unknown Source)
	at org.pojoxml.core.processor.XmlToPojo.parseXMLFromFile(Unknown Source)
	at org.pojoxml.core.PojoXmlImpl.getPojoFromFile(Unknown Source)
	at GenEmployeePOJO.main(GenEmployeePOJO.java:11)
```

## **Examples** ##

> Following are the examples to generate the xml files from POJO and xml back to POJO. In order to run examples you have to set classpath to pojoxml.jar. A complete example source you can find with PojoXML downloaded version

**Simple POJO to xml creation
```
//POJO class for generating xml file


public class Employee {

	// tags are generated based on the variables.
	private String name;
	private int age;
	private double salary;
	private Address address;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public void setAddress(Address address) {
		this.address = address;
	}
	public Address getAddress() {
		return address;
	}
}

```**

```
//Program for creating xml from above pojo.
import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;

public class GenEmployeeXml {

	
	public static void main(String[] args) {
		PojoXml pojoxml = PojoXmlFactory.createPojoXml();

		Employee employee = new Employee();
		Address address = new Address();
		
 		employee.setName("Mike");
		employee.setAge(45);
		employee.setSalary(20000.45);
		address.setAddress1("Sagar");
		address.setAddress2("Nerul");
		address.setCity("Mumbai");
		address.setZip(002345);
		employee.setAddress(address);
		String xml = pojoxml.getXml(employee);
		System.out.println(xml);

	}

}
 
```

```
Generated xml file
<?xml version="1.0" encoding="UTF-8"?>
<Employee>
  <name>Mike</name>
  <age>45</age>
  <salary>20000.45</salary>
  <address>
    <address1>Sagar</address1>
    <address2>Nerul</address2>
    <city>Mumbai</city>
    <zip>1253</zip>
  </address>
</Employee>
```

## **Xml back to Pojo example** ##
```
import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;


public class GenEmployeePOJO {

	public static void main(String args[]){
 		PojoXml pojoXml = PojoXmlFactory.createPojoXml();
		
		 pojoXml.addClassAlias(Employee.class, "EmployeeInfo");
 	 	Employee employee = (Employee)pojoXml.getPojoFromFile("C:\\employee.xml",Employee.class);
	 	
	 	System.out.println(employee.getName());
	}
}
 
```

## **Collection example** ##
```
import java.util.ArrayList;


public class Employee {

	// tags are generated based on the variables.
	private String name;
	private int age;
	private double salary;
	private ArrayList address;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public void setAddress(ArrayList address) {
		this.address = address;
	}
	public ArrayList getAddress() {
		return address;
	}
 }

```

```
import java.util.ArrayList;

import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;

public class GenEmployeeXml {

	
	public static void main(String[] args) {
		PojoXml pojoxml = PojoXmlFactory.createPojoXml();
		ArrayList list = new ArrayList();
		Employee employee = new Employee();
		
		employee.setName("Mike");
		employee.setAge(45);
		employee.setSalary(20000.45);
	    
		list.add(getAddress());
		list.add(getAddress());
		list.add(getAddress());
		employee.setAddress(list);
		String xml = pojoxml.getXml(employee);
		System.out.println(xml);

	}
	
	public static Address getAddress(){
		Address address = new Address();
		address.setAddress1("Sagar");
		address.setAddress2("Nerul");
		address.setZip(002345);
		address.setCity("Mumbai");
		return address;
	}

}
 

Generated xml file
<?xml version="1.0" encoding="UTF-8"?>
<Employee>
  <name>Mike</name>
  <age>45</age>
  <salary>20000.45</salary>
  <address>
    <Address>
      <address1>Sagar</address1>
      <address2>Nerul</address2>
      <city>Mumbai</city>
      <zip>1253</zip>
    </Address>
    <Address>
      <address1>Sagar</address1>
      <address2>Nerul</address2>
      <city>Mumbai</city>
      <zip>1253</zip>
    </Address>
    <Address>
      <address1>Sagar</address1>
      <address2>Nerul</address2>
      <city>Mumbai</city>
      <zip>1253</zip>
    </Address>
  </address>
</Employee>
```

## **xml back to collection object** ##
```
import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;


public class GenEmployeePOJO {

	public static void main(String args[]){
 		PojoXml pojoXml = PojoXmlFactory.createPojoXml();
		//spcify type of the object containes in collection class
 		pojoXml.addCollectionClass("Address",Address.class);
 		Employee employee = (Employee)pojoXml.getPojoFromFile("C:\\employee.xml",Employee.class);
	  
 		System.out.println(employee.getName());
	}
}
```

## **Alias name example** ##
```
import java.util.ArrayList;

import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;

public class GenEmployeeXml {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		PojoXml pojoxml = PojoXmlFactory.createPojoXml();
		ArrayList list = new ArrayList();
		Employee employee = new Employee();
		pojoxml.addClassAlias(Employee.class, "EmployeeInfo");
		pojoxml.addFieldAlias(Employee.class, "name", "employeeName");
		pojoxml.addFieldAlias(Employee.class, "address","employeeAddress");
		pojoxml.addClassAlias(Address.class, "employeeAddess");
		pojoxml.addFieldAlias(Address.class, "address2", "townName");
		employee.setName("Mike");
		employee.setAge(45);
		employee.setSalary(20000.45);
	    
		list.add(getAddress());
		list.add(getAddress());
		list.add(getAddress());
		employee.setAddress(list);
		
		String xml = pojoxml.getXml(employee);
		System.out.println(xml);

	}
	
	public static Address getAddress(){
		Address address = new Address();
		address.setAddress1("Sagar");
		address.setAddress2("Nerul");
		address.setZip(002345);
		address.setCity("Mumbai");
		return address;
	}

}


<?xml version="1.0" encoding="UTF-8"?>
<EmployeeInfo>
  <employeeName>Mike</employeeName>
  <age>45</age>
  <salary>20000.45</salary>
  <employeeAddress>
    <employeeAddess>
      <address1>Sagar</address1>
      <townName>Nerul</townName>
      <city>Mumbai</city>
      <zip>1253</zip>
    </employeeAddess>
    <employeeAddess>
      <address1>Sagar</address1>
      <townName>Nerul</townName>
      <city>Mumbai</city>
      <zip>1253</zip>
    </employeeAddess>
    <employeeAddess>
      <address1>Sagar</address1>
      <townName>Nerul</townName>
      <city>Mumbai</city>
      <zip>1253</zip>
    </employeeAddess>
  </employeeAddress>
</EmployeeInfo>
```

```
import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;


public class GenEmployeePOJO {

	public static void main(String args[]){
 		PojoXml pojoxml = PojoXmlFactory.createPojoXml();
 		//specifying alias name
 		pojoxml.addClassAlias(Employee.class, "EmployeeInfo");
		pojoxml.addFieldAlias(Employee.class, "name", "employeeName");
		pojoxml.addFieldAlias(Employee.class, "address","employeeAddress");
		pojoxml.addClassAlias(Address.class, "employeeAddess");
		pojoxml.addFieldAlias(Address.class, "address2", "townName");
		//spcify type of the object containes in collection class
		pojoxml.addCollectionClass("employeeAddess",Address.class);
 		Employee employee = (Employee)pojoxml.getPojoFromFile("C:\\employee.xml",Employee.class);
	  
 		System.out.println(employee.getName());
	}
}
```

## **Array example** ##
```
Arrays Example



public class Employee {

	// tags are generated based on the variables.
	private String name;
	private int age;
	private double salary;
            //array of objects
	private Address[] address;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public void setAddress(Address[] address) {
		this.address = address;
	}
	public Address[] getAddress() {
		return address;
	}
	 
 }
```

```
import java.util.ArrayList;

import org.pojoxml.core.PojoXml;
import org.pojoxml.core.PojoXmlFactory;

public class GenEmployeeXml {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		PojoXml pojoxml = PojoXmlFactory.createPojoXml();
		ArrayList list = new ArrayList();
		Employee employee = new Employee();
		 
		employee.setName("Mike");
		employee.setAge(45);
		employee.setSalary(20000.45);
	    
		list.add(getAddress());
		list.add(getAddress());
		list.add(getAddress());
		employee.setAddress(new Address[]{getAddress(),getAddress()});
		
		String xml = pojoxml.getXml(employee);
		System.out.println(xml);

	}
	
	public static Address getAddress(){
		Address address = new Address();
		address.setAddress1("Sagar");
		address.setAddress2("Nerul");
		address.setZip(002345);
		address.setCity("Mumbai");
		return address;
	}

}

```

```
<?xml version="1.0" encoding="UTF-8"?>
<Employee>
  <name>Mike</name>
  <age>45</age>
  <salary>20000.45</salary>
  <address>
    <address1>Sagar</address1>
    <address2>Nerul</address2>
    <city>Mumbai</city>
    <zip>1253</zip>
  </address>
  <address>
    <address1>Sagar</address1>
    <address2>Nerul</address2>
    <city>Mumbai</city>
    <zip>1253</zip>
  </address>
</Employee>
```









