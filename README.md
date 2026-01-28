Part 2 — Playwright Automation
playwright.config.ts
import { defineConfig } from '@playwright/test';


export default defineConfig({
  testDir: './tests',
  timeout: 30000,
  use: {
    headless: true,
    baseURL: 'https://claude.ai/public/artifacts/1e02a9a5-4f20-4f19-a7ba-6c3f16c6eab9',
  },
});
Test 1: delayed-button.spec.ts
import { test, expect } from '@playwright/test';


test('Delayed Button Flow', async ({ page }) => {
  await page.goto('/');
  await page.getByText('Timing Challenges').click();
  await page.getByRole('button', { name: 'Start Process' }).click();


  const confirmBtn = page.getByRole('button', { name: 'Confirm Action' });
  await expect(confirmBtn).toBeEnabled();
  await confirmBtn.click();


  await expect(page.getByText('Success')).toBeVisible();
});
Test 2: lazy-list.spec.ts
import { test, expect } from '@playwright/test';


test('Load and verify list items', async ({ page }) => {
  await page.goto('/');
  await page.getByText('Timing Challenges').click();


  for (let i = 0; i < 3; i++) {
    await page.getByRole('button', { name: 'Load More Items' }).click();
    await page.waitForSelector('.loading', { state: 'hidden' });
  }


  const items = page.locator('.list-item');
  await expect(items).toHaveCount(15);


  await expect(items.filter({ hasText: 'active' })).toHaveCountGreaterThan(0);
  await expect(items.filter({ hasText: 'pending' })).toHaveCountGreaterThan(0);
});
Test 3: dynamic-ids.spec.ts
import { test, expect } from '@playwright/test';


test('Dynamic ID handling', async ({ page }) => {
  await page.goto('/');
  await page.getByText('Flaky Selectors').click();


  await page.getByRole('button', { name: 'Regenerate All IDs' }).click();
  await page.getByRole('option', { name: 'Beta' }).click();


  await expect(page.getByText('Selected: Beta')).toBeVisible();
});
Test 4: conditional-render.spec.ts
import { test, expect } from '@playwright/test';


test('Conditional login flow', async ({ page }) => {
  await page.goto('/');
  await page.getByText('Flaky Selectors').click();


  await page.getByRole('button', { name: 'Admin User' }).click();
  await expect(page.getByText('Admin Panel')).toBeVisible();
  await expect(page.getByText('Standard Panel')).toBeHidden();


  await page.getByRole('button', { name: 'Logout' }).click();


  await page.getByRole('button', { name: 'Standard User' }).click();
  await expect(page.getByText('Standard Panel')).toBeVisible();
  await expect(page.getByText('Admin Panel')).toBeHidden();
});
Test 5: modal-flow.spec.ts
import { test, expect } from '@playwright/test';


test('Modal confirmation flow', async ({ page }) => {
  await page.goto('/');
  await page.getByText('Responsive').click();


  await page.getByRole('button', { name: 'Open Modal' }).click();
  const modal = page.locator('.modal').first();


  await modal.getByRole('button', { name: 'Show Details' }).click();
  const nestedModal = page.locator('.modal').nth(1);


  await nestedModal.getByRole('button', { name: 'Confirm' }).click();


  await expect(modal).toBeHidden();
  await expect(page.getByText('confirmed')).toBeVisible();
});
package.json
