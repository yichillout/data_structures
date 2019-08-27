public class Client {
	public static void main(String[] args) throws InterruptedException {
		System.setProperty("webdriver.chrome.driver", "/Users/jasper/Applications/chromedriver");
		ChromeOptions chromeOptions = new ChromeOptions();
		Dimension d = new Dimension(1920, 720);
		WebDriver driver = new ChromeDriver(chromeOptions);
		Actions actions = new Actions(driver);

		driver.get("https://www.amazon.com/");
		driver.manage().timeouts().implicitlyWait(20, TimeUnit.SECONDS);
		// driver.manage().window().maximize();
		File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
		Thread.sleep(2000);
		try {
			FileUtils.copyFile(src, new File("a.png"));
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

//		WebElement webElement = driver
//				.findElement(By.xpath("//*[@id=\"g-mainbar\"]/div[1]/div/div/div/div/div/div[2]/p[18]"));
		// actions.moveToElement(webElement).build();

		// Screenshot fpScreenshot = new AShot().coordsProvider(new
		// WebDriverCoordsProvider()).takeScreenshot(driver,webElement);

		Screenshot fpScreenshot = new AShot().shootingStrategy(ShootingStrategies.viewportPasting(1000))
				.coordsProvider(new WebDriverCoordsProvider()).takeScreenshot(driver);

		// Screenshot fpScreenshot = new AShot().coordsProvider(new
		// WebDriverCoordsProvider()).takeScreenshot(driver);

		// Screenshot fpScreenshot = new AShot().takeScreenshot(driver);

		try {
			ImageIO.write(fpScreenshot.getImage(), "PNG", new File("b.png"));
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		WebDriver augmentedDriver = new Augmenter().augment(driver);
		File screenshot = ((TakesScreenshot) augmentedDriver).getScreenshotAs(OutputType.FILE);
		try {
			FileUtils.copyFile(src, new File("c.png"));
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		driver.quit();

	}
}
