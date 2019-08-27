package com.jasper;

import java.io.File;
import java.io.IOException;
import java.util.concurrent.TimeUnit;

import javax.imageio.ImageIO;

import org.apache.commons.io.FileUtils;
import org.openqa.selenium.By;
import org.openqa.selenium.Dimension;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.remote.Augmenter;

import ru.yandex.qatools.ashot.AShot;
import ru.yandex.qatools.ashot.Screenshot;
import ru.yandex.qatools.ashot.coordinates.WebDriverCoordsProvider;
import ru.yandex.qatools.ashot.shooting.ShootingStrategies;
import ru.yandex.qatools.ashot.shooting.cutter.CutStrategy;

public class Client {
	public static void main(String[] args) throws InterruptedException {
		System.setProperty("webdriver.chrome.driver", "/Users/jasper/Applications/chromedriver");
		ChromeOptions chromeOptions = new ChromeOptions();
		Dimension d = new Dimension(1920, 720);
		WebDriver driver = new ChromeDriver(chromeOptions);
		Actions actions = new Actions(driver);

		driver.get("https://www.amazon.com/");
		// driver.manage().timeouts().implicitlyWait(20, TimeUnit.SECONDS);
		driver.manage().window().maximize();
		// File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
		// Thread.sleep(2000);
		// try {
		// FileUtils.copyFile(src, new File("a.png"));
		// } catch (IOException e) {
		// // TODO Auto-generated catch block
		// e.printStackTrace();
		// }

		// WebElement webElement = driver
		// .findElement(By.xpath("//*[@id=\"g-mainbar\"]/div[1]/div/div/div/div/div/div[2]/p[18]"));
		// actions.moveToElement(webElement).build();

		// Screenshot fpScreenshot = new AShot().coordsProvider(new
		// WebDriverCoordsProvider()).takeScreenshot(driver,webElement);

		Screenshot fpScreenshot = new AShot()
				.shootingStrategy(ShootingStrategies.viewportPasting(ShootingStrategies.scaling(2f), 1000))
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
			FileUtils.copyFile(screenshot, new File("c.png"));
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		driver.quit();

	}
}
