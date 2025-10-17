package com.example;

import com.microsoft.playwright.*;
import com.microsoft.playwright.options.WaitForSelectorState;
import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

public class AuthTest {
    private static Playwright playwright;
    private static Browser browser;
    private BrowserContext context;
    private Page page;

    @BeforeAll
    static void setUpAll() {
        playwright = Playwright.create();
        browser = playwright.chromium().launch(new BrowserType.LaunchOptions().setHeadless(false));
    }

    @BeforeEach
    void setUp() {
        context = browser.newContext();
        page = context.newPage();
        page.navigate("https://dobro.ru/register?__target_path=/dashboard");
    }

    @AfterEach
    void tearDown() {
        context.close();
    }

    @AfterAll
    static void tearDownAll() {
        browser.close();
        playwright.close();
    }

    @Test
    @DisplayName("Успешный вход с валидными данными")
    void successfulLogin() {
        page.waitForSelector("form");

        Locator cookiesButton = page.locator(".CookiesModal_cookies__gR2tZ button");
        cookiesButton.waitFor(new Locator.WaitForOptions().setState(WaitForSelectorState.VISIBLE));
        cookiesButton.click();

        page.fill("input[name=\"firstName\"]", "Тест");
        page.fill("input[name=\"lastName\"]", "Тестов");

        page.click("div.css-1wy0on6");
        page.keyboard().type("Австралия");
        page.keyboard().press("Enter");

        Locator selectedValue = page.locator("div[class*='singleValue']");
        selectedValue.waitFor(new Locator.WaitForOptions().setState(WaitForSelectorState.VISIBLE));
        String actual = selectedValue.textContent().trim();
        assertEquals("Австралия", actual);

        Locator cityInp = page.locator("div.Inputs_form-container__f0FZC > div > div > input");
        cityInp.click();
        cityInp.pressSequentially("Сидн", new Locator.PressSequentiallyOptions().setDelay(100));
        Locator suggestionList = page.locator("text=Новый Южный Уэльс");
        suggestionList.waitFor(new Locator.WaitForOptions().setState(WaitForSelectorState.VISIBLE));
        suggestionList.click();

        page.click("div:nth-child(5)");
        page.keyboard().type("11.04.2000");
        page.keyboard().press("Enter");

        page.click("input[type='email']");
        page.keyboard().type("example120953@mail.ru");


        page.evaluate("window.scrollTo(0, document.body.scrollHeight)");

        page.click("div:nth-child(7)");
        page.keyboard().type("9124226609");


        page.locator("input[type='password']").first().fill("Test12*");
        page.locator("input[type='password']").nth(1).fill("Test12*");
        page.keyboard().press("Enter");

        page.click("div:nth-child(10)");
        page.click("text=Зарегистрироваться");

        page.waitForSelector("text=Проверьте электронную почту",
                new Page.WaitForSelectorOptions().setState(WaitForSelectorState.VISIBLE));
        assertTrue(page.isVisible("text=Проверьте электронную почту"),
                "Регистрация не завершилась успешно");
    }
}
