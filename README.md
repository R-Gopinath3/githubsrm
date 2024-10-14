6.	CODING AND TESTING

6.1	MAIN CLASS:

package testpack; import java.io.File; import java.net.URL;
import java.text.SimpleDateFormat; import java.util.Date;
import java.util.Properties;
import java.util.concurrent.TimeUnit; import java.time.Duration;
import java.util.ArrayList; import java.util.HashSet; import java.util.List; import java.util.Set;
import javax.mail.Message;
import javax.mail.MessagingException; import javax.mail.PasswordAuthentication; import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress; import javax.mail.internet.MimeMessage; import org.apache.commons.io.FileUtils; import org.openqa.selenium.By;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver; import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver; import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.support.ui.ExpectedConditions; import org.openqa.selenium.support.ui.WebDriverWait; import com.aventstack.extentreports.Status;
import org.openqa.selenium.OutputType; import org.openqa.selenium.TakesScreenshot; import org.openqa.selenium.WebDriver; import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver; import org.openqa.selenium.chrome.ChromeOptions; import org.openqa.selenium.remote.DesiredCapabilities; import org.openqa.selenium.remote.RemoteWebDriver; import org.testng.Assert;
import com.aventstack.extentreports.ExtentReports; import com.aventstack.extentreports.ExtentTest; import com.aventstack.extentreports.Status;
import com.aventstack.extentreports.reporter.ExtentHtmlReporter;
import com.aventstack.extentreports.reporter.configuration.ChartLocation; import com.aventstack.extentreports.reporter.configuration.Theme;
 
import io.appium.java_client.android.AndroidDriver; public class FacebookAutomation{

//public static WebDriver driver;
//public static AndroidDriver driver;
public static ExtentHtmlReporter htmlReporter; public static ExtentReports extent;
public static ExtentTest test; public static String folderPath; public static String filename; public static String foldername;
public static void main(String[] args) throws Exception {

test = extent.createTest("Automation"); System.setProperty("webdriver.chrome.driver","C:\\Users\\Gopinath\\eclipse-
workspace\\test11111\\chrome driver\\chromedriver.exe");

ChromeOptions options = new ChromeOptions(); options.addArguments("--remote-allow-origins=*"); driver = new ChromeDriver(options);

driver.manage().window().maximize();

driver.get("https://www.geeksforgeeks.org/"); test.log(Status.PASS, "Navigated to geeks"); try
{
driver.findElement(By.xpath("//input[@class='ant-input ant-input- lgaaa']")).sendKeys("testing");
test.log(Status.PASS, "Successfully entered the search text");
}
catch(Exception e)
{
test.log(Status.FAIL, "search text is not entered"); takeScreenshot("xpath issue");
}
extent.flush(); driver.quit(); setUp(); sendmail();
}

public static void sendmail()
{
final String user="manokaran.sr@gmail.com";//change accordingly final String password="srivaishu";//change accordingly
String to="saianiruthan@gmail.com";//change accordingly

//Get the session object
Properties props = new Properties();
 
props.setProperty("mail.transport.protocol", "smtp"); props.setProperty("mail.host", "smtp.gmail.com"); props.put("mail.smtp.auth", "true");
props.put("mail.smtp.port", "465");
props.put("mail.debug", "true"); props.put("mail.smtp.socketFactory.port", "465");
props.put("mail.smtp.socketFactory.class","javax.net.ssl.SSLSocketFactory"); props.put("mail.smtp.socketFactory.fallback", "false");
Session session = Session.getDefaultInstance(props, new javax.mail.Authenticator() {
protected PasswordAuthentication getPasswordAuthentication() { return new PasswordAuthentication(user,password);
}
});

//Compose the message try {

Transport transport = session.getTransport(); InternetAddress addressFrom = new InternetAddress(user);
MimeMessage message = new MimeMessage(session); message.setFrom(new InternetAddress(user)); message.addRecipient(Message.RecipientType.TO,new InternetAddress(to)); message.setSender(addressFrom);
message.setSubject("javatpoint");
message.setText("This is simple program of sending email using JavaMail API");

//send the message transport.connect(); Transport.send(message); transport.close();


System.out.println("message sent successfully...");

} catch (MessagingException e) {e.printStackTrace();}
}


public static void setUp()
{

String dateName = new SimpleDateFormat("yyyy"+"-"+"MM"+"-"+"dd"+"- "+"hh"+"-"+"mm"+"-"+"ss").format(new Date());
foldername = "Naruto_"+dateName;
File dir1 = new File(System.getProperty("user.dir")+"\\TestReport\\"+foldername);
//Specify the Folder name here
dir1.mkdir(); //Creates the folder with the above specified name folderPath = dir1.getAbsolutePath();
filename = "Naruto_"+dateName+".html";
 
htmlReporter = new ExtentHtmlReporter(folderPath+"/"+filename); extent = new ExtentReports();
extent.attachReporter(htmlReporter); extent.setSystemInfo("Project", "Mobile testing"); extent.setSystemInfo("QA Tester", "Naruto"); extent.setSystemInfo("Environment", "Test FaceBook"); extent.setSystemInfo("Framework", "NULL"); extent.setSystemInfo("Organization", "Group31");

htmlReporter.config().setChartVisibilityOnOpen(true); htmlReporter.config().setDocumentTitle("Test Execution Summary Report"); htmlReporter.config().setReportName("FB Reports"); htmlReporter.config().setTestViewChartLocation(ChartLocation.TOP); htmlReporter.config().setTheme(Theme.STANDARD);
}

//Capture Screenshot and save it in saved location
public static String getScreenshot(WebDriver driver, String screenshotName) throws Exception {
String dateName = new SimpleDateFormat("yyyyMMddhhmmss").format(new
 
Date());
 

TakesScreenshot ts = (TakesScreenshot) driver;
File source = ts.getScreenshotAs(OutputType.FILE);
//pass the relative path in the html report String htmlrelativepath = "../"+foldername +
 
"/"+screenshotName+"_"+dateName+".png";
//to move the screenshot taken to the report folder
String destination = folderPath+ "/"+screenshotName+"_"+dateName+".png"; File finalDestination = new File(destination);
FileUtils.copyFile(source, finalDestination); return htmlrelativepath;
}

//take screenshot
public static void takeScreenshot(String stepname) throws Exception
{
String screenshotpath = getScreenshot(driver,stepname); test.addScreenCaptureFromPath(screenshotpath);
}

public static WebDriver driver; public static int count;
public static void launch(String url){

System.setProperty("webdriver.chrome.driver","./driver/chromedriver.exe"); driver = new ChromeDriver();
driver.manage().timeouts().implicitlyWait(25,TimeUnit.SECONDS); driver.manage().window().maximize();
driver.get(url);}
public static void Wait(int Time) throws InterruptedException{
 
Thread.sleep(Time);}
public static void clickBtn(WebElement Element){ Element.click();}
public static void sendkeys(WebElement element, String value){ element.clear();
element.sendKeys(value);}
public static WebElement waitTillElementlickable(WebDriver driver,WebElement webElement,int seconds, String Command)
{
 
try
{

Duration.ofSeconds(seconds));
 

WebDriverWait wait = new WebDriverWait(driver, WebElement a
 
=wait.until(ExpectedConditions.elementToBeClickable(webElement));
System.out.println(Command); return a;
}
catch (Exception e)
{
 


finally
{
 
System.out.println("unable to wait till elemn=ent is clicked");
//status = false;
}
 

driver.manage().timeouts().implicitlyWait(20,TimeUnit.SECONDS);
}
return webElement;
}
public static WebElement waitTillElementvisible(WebDriver driver,WebElement webElement1,int seconds, String Command)
{
try
{
WebDriverWait wait = new WebDriverWait(driver,
Duration.ofSeconds(seconds));
WebElement b = wait.until(ExpectedConditions.visibilityOf(webElement1));
System.out.println(Command); return b;
}
catch (Exception e)
{
 

false;
 


finally
{
 
System.out.println("unable to wait till elemn=ent is clicked");//status =

}


driver.manage().timeouts().implicitlyWait(20,TimeUnit.SECONDS);
 
}
return webElement1;
}
public static void getText(WebElement element)
{
List<WebElement> elements = driver.findElements(By.xpath("//div[@class = 'hmenu-item hmenu-title ']"));
System.out.println(elements.size()); for (int i = 0; i < elements.size(); i++)
//Formatter formatter = new Formatter();
{
WebElement elements1 = elements.get(i); String text = elements1.getText(); count=count+1;


ArrayList<String> list=new ArrayList<String>();//(Arrays.asList(input.split(";"))) ;
list.add(text);
list.remove("Shop By Department");
//	list.forEach(System.out::println);
}
}
}

6.2	WEB APP AUTOMATION TESTING

package testpack;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver; import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver; import org.openqa.selenium.chrome.ChromeOptions; import com.aventstack.extentreports.Status;

public class Automation extends FacebookAutomation
{
public static WebDriver driver;
public static void main(String[] args)throws Exception
{
test = extent.createTest("Automation"); System.setProperty("webdriver.chrome.driver","C:\\Users\\Gopinath\\eclipse-
workspace\\test11111\\chrome driver\\chromedriver.exe"); ChromeOptions options = new ChromeOptions(); options.addArguments("--remote-allow-origins=*"); driver = new ChromeDriver(options); driver.manage().window().maximize();
driver.get("https://sprightly-cascaron-28998e.netlify.app/"); test.log(Status.PASS, "Navigated to geeks");
try
 
{
Thread.sleep(5000); driver.findElement(By.xpath("//select//option[text()='INR']")).click(); test.log(Status.PASS, "Successfully entered the search text");
}
catch(Exception e)
{
test.log(Status.FAIL, "search text is not entered"); takeScreenshot("xpath issue");
}
driver.close();
}
}


6.3	MOBILE APP TESTING

package testpack;

import java.net.URL;

import org.openqa.selenium.By;

import org.openqa.selenium.WebElement;

import org.openqa.selenium.remote.DesiredCapabilities; import org.openqa.selenium.remote.RemoteWebDriver; import org.testng.Assert;
import io.appium.java_client.android.AndroidDriver;

public class MobileAppTest {

public static AndroidDriver driver1;

public static void main(String[] args) throws Exception {

//set up desired capabilities

DesiredCapabilities capabilities = new DesiredCapabilities(); capabilities.setCapability("deviceName", "Naruto"); capabilities.setCapability("platformName", "Android"); capabilities.setCapability("platformVersion", "12.0"); capabilities.setCapability("udid", "d6301c81");
 
capabilities.setCapability("appPackage", "com.facebook.katana"); capabilities.setCapability("appActivity", "com.facebook.katana.LoginActivity");
//Create a new RemoteWebDriver

RemoteWebDriver driver = new RemoteWebDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);

//Enter the username and password

WebElement userName =
driver1.findElement(By.id("com.facebook.katana:id/login_username")); userName.sendKeys("username");
WebElement password =
driver1.findElement(By.id("com.facebook.katana:idlogin_password")); password.sendKeys("password");
//Click on the Login button

WebElement loginButton =
driver1.findElement(By.id("com.facebook.katana:id/login_login")); loginButton.click();
//Verify the login was successful

WebElement homeButton =
driver1.findElement(By.id("com.facebook.katana:id/home_fragment_title")); Assert.assertTrue(homeButton.isDisplayed());
//Close the driver driver.quit();
}

}
 
6.4	REPORT AND EMAIL

package testEmail;

import java.util.Properties;

import javax.mail.*;

import javax.mail.internet.*;

public class testEm {

public static void main(String[] args) {

final String user="manokaran.sr@gmail.com";//change accordingly

final String password="srivaishu";//change accordingly String to="saianiruthan@gmail.com";//change accordingly
//Get the session object

Properties props = new Properties(); props.setProperty("mail.transport.protocol", "smtp"); props.setProperty("mail.host", "smtp.gmail.com"); props.put("mail.smtp.auth", "true");
props.put("mail.smtp.port", "465");

props.put("mail.debug", "true"); props.put("mail.smtp.socketFactory.port", "465");
props.put("mail.smtp.socketFactory.class","javax.net.ssl.SSLSocketFactory"); props.put("mail.smtp.socketFactory.fallback", "false");
Session session = Session.getDefaultInstance(props,

new javax.mail.Authenticator() {

protected PasswordAuthentication getPasswordAuthentication() {

return new PasswordAuthentication(user,password); }});

//Compose the message
 
try {

Transport transport = session.getTransport(); InternetAddress addressFrom = new InternetAddress(user); MimeMessage message = new MimeMessage(session); message.setFrom(new InternetAddress(user));
message.addRecipient(Message.RecipientType.TO,new InternetAddress(to)); message.setSender(addressFrom);
message.setSubject("javatpoint");

message.setText("This is simple program of sending email using JavaMail API");

//send the message transport.connect(); Transport.send(message); transport.close();
System.out.println("message sent successfully...");

} catch (MessagingException e) {e.printStackTrace();

}

}

}
