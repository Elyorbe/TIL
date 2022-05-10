## Core annotations

- *A test class* may be a top-level class, static member clas, or an inner class annotated with as `@Nested` that contains one or more test methods.
Test classes cannot be abstract and must have single no args constructor, or with args that can be dynamically resolved at runtime through dependency injection.

- *A test method* is an instance method that is annotated with `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`, or `@TestTemplate`.

- *A life cycle method* is a method that is annotated with `@BeforeAll`, `@AfterAll`, `@BeforeEach`, `@AfterEach`.

Test methods must not be abstract and must not return a value.

Junit creates a new instance of the test class before invoking each `@Test` method to ensure the independence of the test methods and prevent unintentional side effects
in the test code. If a test class is annotated with `@TestInstance(Lifecycle.PER_CLASS`, JUnit 5 will excecute all test methods on the same test instance.


```java
class SUTTest {
        private static ResourceForAllTests resourceForAllTests;
        private SUT systemUnderTest;
        
        // Executed once: Before all tests. Must be static
        @BeforeAll
        static void setUpClass() {
            resourceForAllTests =
                    new ResourceForAllTests("Our resource for all tests");
        }
        
        // Executed once: After all tests. Must be static
        @AfterAll 
        static void tearDownClass() {
            resourceForAllTests.close();
        }
        
        // Executed before each test
        @BeforeEach
        void setUp() {
            systemUnderTest = new SUT("Our system under test");
        }
        
        // Executed after each test
        @AfterEach
        void tearDown() {
            systemUnderTest.close();
        }
        
        // Executed independently
        @Test
        void testRegularWork() {
            boolean canReceiveRegularWork =
                    systemUnderTest.canReceiveRegularWork();
            assertTrue(canReceiveRegularWork);
        }
        
        // Executed independently
        @Test
        void testAdditionalWork() {
            boolean canReceiveAdditionalWork =
                    systemUnderTest.canReceiveAdditionalWork();
            assertFalse(canReceiveAdditionalWork);
        }
    }
```

`@DisplayName` annotation can be used over classes and test methods to declare their own display name. Can be useful for test reporting in IDEs.

`@Disabled` annotation signals a class or a test method not to execute tests. When disabling tests reasons should be provided. Such as *Feature is still under
construction*.

`@Nested` annotation can be used for inner classes. The typical use case is when two classes are tightly coupled.

`@Tag` annotation can be used over classes and methods to filter test discovery and execution.


## Assertions
To perform test validation, assert methods from JUnit `Assertions` class.
Some of the most used assert methods are: `asssertTrue, assertFalse, assertEqual, assertNull`. Method execution time can be tested using `assertTimeoutX` methods.
If we expect a test to be executed and to throw an exception, we can make use of `assertThrows` methods.
