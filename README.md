Download Link: https://assignmentchef.com/product/solved-cs5200-introduction-to-relational-databases-mongodb-assignment
<br>
<span style="font-size: 2.61792em; letter-spacing: -1px; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;">Introduction</span>

Consider the class diagram below. This assignment will map the following classes to an equivalent Mongo database:  <strong>Student </strong>,  <strong>Answer </strong>,  <strong>Question </strong>,  <strong>MultipleChoice </strong>,  <strong>TrueFalse </strong>,  <strong>Question </strong>,  <strong>QuizWidget </strong>. Ignore all other classes. Feel free to add additional fields, but do not rename the existing variable names.







Create and connect to a MongoDB database

In file  <strong>/data/db.js</strong> use Mongoose.js to connect to a database called  <strong>white-board </strong>.

<h1>Implement Mongoose schemas</h1>

It is a good practice to implement Mongoose schemas in their own files allowing them to be reused by and defined in terms of other schemas. Create the following schemas in directory  <strong>/data/models/</strong> based on the class diagram provided. Instead of letting MongoDB create the  <strong>_id</strong> unique identifiers for you, instead declare your own  <strong>_id</strong> attribute in your schemas as Number. This will allow using our own IDs which will simplify testing. Use equivalent JavaScript and Mongoose data types where appropriate. Feel free to make any changes/additions where necessary.




<table width="719">

 <tbody>

  <tr>

   <td width="150"><strong>Class </strong></td>

   <td width="270"><strong>Mongoose schema file </strong></td>

   <td width="299"><strong>MongoDB collection </strong></td>

  </tr>

  <tr>

   <td width="150">Student</td>

   <td width="270">student.schema.server.js</td>

   <td width="299">students</td>

  </tr>

  <tr>

   <td width="150">MultipleChoice</td>

   <td width="270">multiple-choice.schema.server.js</td>

   <td width="299">N/A</td>

  </tr>

  <tr>

   <td width="150">TrueFalse</td>

   <td width="270">true-false.schema.server.js</td>

   <td width="299">N/A</td>

  </tr>

 </tbody>

</table>

<h1>Implement inheritance schema</h1>

Implement base class  <strong>Question</strong> and derived classes  <strong>TrueFalse</strong> and  <strong>MultipleChoice</strong> as an inheritance relationship implemented using a denormalized strategy, i.e., use a single schema to represent all three classes in file  <strong>/data/models/question.schema.server.js </strong>. Use a discrimination field called  <strong>questionType</strong> with required enumerated values ‘ <strong>MULTIPLE_CHOICE </strong>’ and ‘ <strong>TRUE_FALSE </strong>’ to distinguish between the question types. Instead of the schema for the Question class having the fields of  <strong>MultipleChoice</strong> and  <strong>TrueFalse</strong> classes, model these as embedded schemas accessible through schema properties  <strong>multipleChoice</strong> and <strong>trueFalse </strong>. Store question instances in a collection called questions. Note the schema is declaring an  <strong>_id</strong> attribute to allow providing our own ID for testing. Feel free to use/modify/ignore the example below as an implementation of the schema.




<table width="720">

 <tbody>

  <tr>

   <td width="720"><strong>/data/models/question.schema.server.js</strong></td>

  </tr>

  <tr>

   <td width="720"><strong>const  </strong>mongoose  = require( <strong>‘mongoose’ </strong>)<strong>const  </strong>TrueFalseSchema  = require( <strong>‘./true­false.schema.server.js’ </strong>)<strong>const  </strong>MultipleChoiceSchema  = require( <strong>‘./multiple­choice.schema.server.js’ </strong>) modules. <strong>exports  </strong>=  mongoose . Schema ({<strong>_id </strong>: Number,<strong>question </strong>: String,<strong>points </strong>: Number,<strong>questionType </strong>: String,<strong>multipleChoice </strong>:  MultipleChoiceSchema ,<strong>trueFalse </strong>:  TrueFalseSchema}, { <strong>collection </strong>:  <strong>‘questions’ </strong>})</td>

  </tr>

 </tbody>

</table>

<h1>Implement one to many relationship</h1>

In schema file  <strong>/data/models/quiz-widget.schema.server.js </strong>, implement the one to many relationship between  <strong>QuizWidget</strong> and  <strong>Question</strong> classes. Use an array of  <strong>ObjectIds</strong> to hold the IDs of the questions that make up the widget. Feel free to use/modify/ignore the example below as an implementation of the schema.




<table width="720">

 <tbody>

  <tr>

   <td width="720"><strong>/data/models/quiz-widget.schema.server.js</strong></td>

  </tr>

  <tr>

   <td width="720"><strong>const  </strong>mongoose  = require( <strong>‘mongoose’ </strong>)</td>

  </tr>

 </tbody>

</table>

<strong>const  </strong>questionSchema  = require( <strong>‘./question.schema.server’ </strong>) <strong>const  </strong>questionWidgetSchema  =  mongoose . Schema ({

<strong>questions </strong>: [{

<strong>type </strong>:  mongoose . Schema . <strong>Types </strong>. <strong>ObjectId </strong>,

<strong>ref </strong>:  <strong>‘QuestionModel’ </strong>

<strong>  </strong>}]

}, { <strong>collection </strong>:  <strong>‘question­widgets’ </strong>})

<h1>Implement many to many</h1>

Implement class  <strong>Answer</strong> as a many to many association between classes  <strong>Student</strong> and  <strong>Question</strong> in Mongoose schema file  <strong>/data/models/answer.schema.server.js </strong>. Feel free to use/modify/ignore the example below as an implementation of the schema.




<table width="720">

 <tbody>

  <tr>

   <td width="720"><strong>/data/models/answer.schema.server.js</strong></td>

  </tr>

  <tr>

   <td width="720"><strong>const  </strong>mongoose  = require( <strong>‘mongoose’ </strong>) <strong>const  </strong>student  = require( <strong>‘./student.schema.server’ </strong>) <strong>const  </strong>question  = require( <strong>‘./question.schema.server’ </strong>) <strong>const  </strong>answer  =  mongoose . Schema ({<strong>_id </strong>: Number,<strong>trueFalseAnswer </strong>: Boolean,<strong>multipleChoiceAnswer </strong>: Number,<strong>student </strong>: { <strong>type </strong>:  mongoose . Schema . <strong>Types </strong>. <strong>ObjectId </strong>,  <strong>ref </strong>:  <strong>‘StudentModel’ </strong>},<strong>question </strong>: { <strong>type </strong>:  mongoose . Schema . <strong>Types </strong>. <strong>ObjectId </strong>,  <strong>ref </strong>:  <strong>‘QuestionModel’ </strong>} }, { <strong>collection </strong>:  <strong>‘answers’ </strong>})</td>

  </tr>

 </tbody>

</table>

<h1>Implement Mongoose data models</h1>

It is a good practice to implement Mongoose models for each collection in a separate file. This allow models to be used by and defined in terms of other models. In directory <strong>/data/models/</strong>  implement mongoose models for the following classes.




<table width="716">

 <tbody>

  <tr>

   <td width="216"><strong>Class </strong></td>

   <td width="33"> </td>

   <td width="467"><strong>File </strong></td>

  </tr>

  <tr>

   <td width="216">Student</td>

   <td width="33"> </td>

   <td width="467">student.model.server.js</td>

  </tr>

  <tr>

   <td width="216">Answer</td>

   <td width="33"> </td>

   <td width="467">answer.model.server.js</td>

  </tr>

  <tr>

   <td width="216">Question</td>

   <td width="33"> </td>

   <td width="467">question.model.server.js</td>

  </tr>

  <tr>

   <td width="216">QuizWidget</td>

   <td width="33"> </td>

   <td width="467">quiz-widget.model.server.js</td>

  </tr>

 </tbody>

</table>




Feel free to use/modify/ignore the example below as an implementation of the student model.







<table width="720">

 <tbody>

  <tr>

   <td width="720"><strong>/data/models/student . model.server.js</strong></td>

  </tr>

  <tr>

   <td width="720"><strong>const  </strong>mongoose  = require( <strong>‘mongoose’ </strong>)<strong>const  </strong>studentSchema  = require( <strong>‘./student.schema.server’ </strong>) module. exports  =  mongoose . model (<strong>‘StudentModel’ </strong>    ,  studentSchema )</td>

  </tr>

 </tbody>

</table>

<h1>Create DAOs for each of the data models</h1>

DAOs (Data Access Objects) encapsulate and modularize interaction with a database by providing a high level interface to the database. DAOs allow other parts of the application to interact with the database without bothering with the idiosyncrasies and minutiae of a particular database implementation or vendor.




In file called  <strong>/data/daos/university.dao.server.js </strong>, create a DAO that implements the following API. If you prefer, feel free to breakup the DAO into several DAO files. The DAOs must use the Mongoose models implemented earlier. Feel free to modify the signature of the methods if appropriate and create additional methods if needed, i.e., you might prefer to pass the IDs of existing documents instead of document instances. Implement the following API:




<ol>

 <li><strong>truncateDatabase()</strong> – removes all the data from the database. Note that you might need to remove documents in a particular order</li>

 <li><strong>populateDatabase()</strong> – populates the database with test data as described in a later section</li>

 <li><strong>createStudent(student)</strong> – inserts a student document</li>

 <li><strong>deleteStudent(id)</strong> – removes student whose ID is id</li>

 <li><strong>createQuestion(question)</strong> – inserts a question document</li>

 <li><strong>deleteQuestion(id)</strong> – removes question whose ID is id</li>

 <li><strong>answerQuestion(studentId, questionId, answer)</strong> – inserts an answer by student student for question question</li>

 <li><strong>deleteAnswer(id)</strong> – removes answer whose ID is id</li>

</ol>




Implement the following finder methods




<ol>

 <li><strong>findAllStudents()</strong> – retrieves all students</li>

 <li><strong>findStudentById(id)</strong> – retrieves a single student document whose ID is id</li>

 <li><strong>findAllQuestions()</strong> – retrieves all questions</li>

 <li><strong>findQuestionById(id)</strong> – retrieves a single question document whose ID is id</li>

 <li><strong>findAllAnswers()</strong> – retrieves all the answers</li>

 <li><strong>findAnswerById(id)</strong> – retrieves a single answer document whose ID is id</li>

 <li><strong>findAnswersByStudent(studentId)</strong> – retrieves all the answers for a student whose ID is studentId</li>

 <li><strong>findAnswersByQuestion(questionId)</strong> – retrieves all the answers for a question whose ID is questionId</li>

</ol>




Feel free to use/modify/ignore the example below as an implementation of the student related DAO functions




<table width="720">

 <tbody>

  <tr>

   <td width="720"><strong>/data/daos/university.dao.server.js</strong></td>

  </tr>

  <tr>

   <td width="720"><strong>const  </strong>studentModel  = require( <strong>‘../models/student.model.server’ </strong>)</td>

  </tr>

 </tbody>

</table>

createStudent  = student =&gt;  studentModel . save (student)

findAllStudents  = () =&gt;  studentModel . find ()

findStudentById  = studentId =&gt;  studentModel . findById (studentId) updateStudent  = (studentId, student) =&gt;  studentModel . update ({<strong>_id </strong>  : studentId}, { <strong>$set </strong>: student}) deleteStudent  = studentId =&gt;  studentModel . remove ({<strong>_id </strong>          : studentId}) module. exports  = {

createStudent ,  findAllStudents ,  findStudentById ,  updateStudent ,  deleteStudent  }

<h1>Populate the database with test data</h1>

In DAO function <strong>populateDatabase() </strong> , populate the database with the data described in the following sections

<h2>Create students</h2>

Use the DAO functions to create the following students. Use the IDs in the  <strong>_id</strong> column as the IDs for the documents inserted.




<table width="717">

 <tbody>

  <tr>

   <td width="54"><strong>_id </strong></td>

   <td width="103"><strong>First Name </strong></td>

   <td width="131"><strong>Last Name </strong></td>

   <td width="121"><strong>Username </strong></td>

   <td width="108"><strong>Password </strong></td>

   <td width="98"><strong>Grad Year </strong></td>

   <td width="101"><strong>Scholarship </strong></td>

  </tr>

  <tr>

   <td width="54">123</td>

   <td width="103">Alice</td>

   <td width="131">Wonderland</td>

   <td width="121">alice</td>

   <td width="108">alice</td>

   <td width="98">2020</td>

   <td width="101">15000</td>

  </tr>

  <tr>

   <td width="54">234</td>

   <td width="103">Bob</td>

   <td width="131">Hope</td>

   <td width="121">bob</td>

   <td width="108">bob</td>

   <td width="98">2021</td>

   <td width="101">12000</td>

  </tr>

 </tbody>

</table>

<h2>Create questions</h2>

Use the DAO functions to create the following questions. Use the IDs in the <strong>_id</strong>  column as the IDs for the documents inserted.




<table width="720">

 <tbody>

  <tr>

   <td width="48"><strong>_id </strong></td>

   <td width="121"><strong>Question </strong></td>

   <td width="61"><strong>Points </strong></td>

   <td width="143"><strong>Type </strong></td>

   <td width="71"><strong>Is true </strong></td>

   <td width="199"><strong>Choices </strong></td>

   <td width="77"><strong>Correct </strong></td>

  </tr>

  <tr>

   <td width="48">321</td>

   <td width="121">Is the following schema valid?</td>

   <td width="61">10</td>

   <td width="143">TRUE_FALSE</td>

   <td width="71">false</td>

   <td width="199"> </td>

   <td width="77"> </td>

  </tr>

  <tr>

   <td width="48">432</td>

   <td width="121">DAO stands forDynamic AccessObject.</td>

   <td width="61">10</td>

   <td width="143">TRUE_FALSE</td>

   <td width="71">false</td>

   <td width="199"> </td>

   <td width="77"> </td>

  </tr>

  <tr>

   <td width="48">543</td>

   <td width="121">What does JPA stand for?</td>

   <td width="61">10</td>

   <td width="143">MULTIPLE_CHOICE</td>

   <td width="71"> </td>

   <td width="199">“Java Persistence API,JavaPersistedApplication,JavaScriptPersistence API,JSONPersistent Associations”</td>

   <td width="77">1</td>

  </tr>

  <tr>

   <td width="48">654</td>

   <td width="121">What doesORM stand for?</td>

   <td width="61">10</td>

   <td width="143">MULTIPLE_CHOICE</td>

   <td width="71"> </td>

   <td width="199">“Object RelationalModel,Object RelativeMarkup,Object ReflexiveModel,Object RelationalMapping”</td>

   <td width="77">4</td>

  </tr>

 </tbody>

</table>

<h2>Create answers</h2>

Use the DAO functions to create the following questions. Use the IDs in the  <strong>_id</strong> column as the IDs for the documents inserted. Use the IDs for students and questions listed in the table below.




<table width="720">

 <tbody>

  <tr>

   <td width="58"><strong>_id </strong></td>

   <td width="74"><strong>Student </strong></td>

   <td width="276"><strong>Question </strong></td>

   <td width="137"><strong>True false answer </strong></td>

   <td width="175"><strong>Multiple Choice Answer </strong></td>

  </tr>

  <tr>

   <td width="58">123</td>

   <td width="74">Alice</td>

   <td width="276">Is the following schema valid?</td>

   <td width="137">true</td>

   <td width="175"> </td>

  </tr>

  <tr>

   <td width="58">234</td>

   <td width="74">Alice</td>

   <td width="276">DAO stands for Dynamic Access Object</td>

   <td width="137">false</td>

   <td width="175"> </td>

  </tr>

  <tr>

   <td width="58">345</td>

   <td width="74">Alice</td>

   <td width="276">What does JPA stand for?</td>

   <td width="137"> </td>

   <td width="175">1</td>

  </tr>

  <tr>

   <td width="58">456</td>

   <td width="74">Alice</td>

   <td width="276">What does ORM stand for?</td>

   <td width="137"> </td>

   <td width="175">2</td>

  </tr>

  <tr>

   <td width="58">567</td>

   <td width="74">Bob</td>

   <td width="276">Is the following schema valid?</td>

   <td width="137">false</td>

   <td width="175"> </td>

  </tr>

  <tr>

   <td width="58">678</td>

   <td width="74">Bob</td>

   <td width="276">DAO stands for Dynamic Access Object</td>

   <td width="137">true</td>

   <td width="175"> </td>

  </tr>

  <tr>

   <td width="58">789</td>

   <td width="74">Bob</td>

   <td width="276">What does JPA stand for?</td>

   <td width="137"> </td>

   <td width="175">3</td>

  </tr>

  <tr>

   <td width="58">890</td>

   <td width="74">Bob</td>

   <td width="276">What does ORM stand for?</td>

   <td width="137"> </td>

   <td width="175">4</td>

  </tr>

 </tbody>

</table>

<h1>Create a test suite</h1>

In a file called  <strong>/test.js </strong>, create tests that validate your DAO, schemas, and models. The test suite should use the DAO functions  <strong>truncateBatabase()</strong> and  <strong>populateDatabase()</strong> to initialize the database to an initial state. It then should implement and use the following functions to validate the database is in the valid state The test.js implement and invoke the following functions in the right order. Note that you might need to use promises to ensure the order of operation.




<ol>

 <li><strong>truncateDatabase()</strong> – uses DAO’s truncateDatabase() function to remove all the data from the database</li>

 <li><strong>populateDatabase()</strong> – uses DAO’s populateDatabase() function to populate the database with test data</li>

 <li><strong>testStudentsInitialCount()</strong> – uses DAO to validate there are 2 students initially</li>

 <li><strong>testQuestionsInitialCount()</strong> – uses DAO to validate there are 4 questions initially</li>

 <li><strong>testAnswersInitialCount()</strong> – uses DAO to validate there are 8 answers initially</li>

 <li><strong>testDeleteAnswer()</strong> – uses DAO to remove Bob’s answer for the multiple choice question “What does ORM stand for?” and validates the total number of questions is 7 and Bob’s total number of questions is 3</li>

 <li><strong>testDeleteQuestion()</strong> – uses DAO to remove true false question “Is the following schema valid?” and validates the total number of questions is 3</li>

 <li><strong>testDeleteStudent()</strong> – uses DAO to remove student Bob and validates the total number of students</li>

</ol>

is 1


