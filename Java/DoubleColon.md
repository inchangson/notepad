# Method Reference operator


~~~
List<String> myList = Arrays.asList("a1", "a2", "b1", "c2", "c1");

//Using Lambda function to call system.out.println()
myList.stream().map(s -> s.toUpperCase())
		.forEach(s -> System.out.println(s));
		
//Using Method reference to call system.out.println()
myList.stream().map(String::toUpperCase).sorted()
		.forEach(System.out::println);
~~~
