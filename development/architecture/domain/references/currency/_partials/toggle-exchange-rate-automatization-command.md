`PrestaShop\PrestaShop\Core\Domain\Currency\Command\ToggleExchangeRateAutomatizationCommand`

{{% notice warning %}}
**Deprecated:** This command previously integrated with the CronJobs module, which has been archived as of December 2025. To automatically update currency exchange rates, you should configure a standard cron job instead. See the [Pricing FAQ]({{< relref "/faq/pricing" >}}) for details on setting up currency rate updates.
{{% /notice %}}

_Class ToggleExchangeRateAutomatizationCommand is responsible for turning on or off the automatic exchange rate update setting._

| Command details            |    |
| -------------------------- | -- |
| **Constructor parameters** | <ul> <li>`$bool $exchangeRateStatus`</li> </ul> |
| **Handler class**          | `PrestaShop\PrestaShop\Adapter\Currency\CommandHandler\ToggleExchangeRateAutomatizationHandler`  <p> Implements: </p> <ul>  <li>`PrestaShop\PrestaShop\Core\Domain\Currency\CommandHandler\ToggleExchangeRateAutomatizationHandlerInterface`</li>  |
| **Return type** |  `void`  |
