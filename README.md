# Home-Assistant City-Mind Water Meter (Israel only)

This is a [Home Assistant](https://www.home-assistant.io/) integration for the Israeli [cp.city-mind.com](https://cp.city-mind.com)
online water meters service that serves many water services.

#### Please ⭐️ this repo if you find it useful

This integration provides Home Assistant with five **sensors** for water consumption in a minimum resolution of 100 liters:

- Last Reading (Current meter reading in a resolution of 100 liters)
- Today's Consumption
- Yesterday's Consumption
- Monthly Consumption
- Consumption Predication

<img src =
  "https://user-images.githubusercontent.com/255973/88915377-d3352d80-d26c-11ea-8ffc-58d7adcca3b5.png"
  height="300" width="451"
  alt="24 hours water meter graph">

This project is not associated in any way with Arad Group or any of its companies that own and operate the City Mind water services.

<a href =
  "https://user-images.githubusercontent.com/255973/87365347-ab607d00-c57e-11ea-9440-19e7805cf9ac.png"
  target = "_blank">
<img src =
  "https://user-images.githubusercontent.com/255973/87365347-ab607d00-c57e-11ea-9440-19e7805cf9ac.png"
  height="300" width="450"
  alt="Water Meter by Arad Technologies">
</a>

## Requirements

You need to sign-up for the service at **[https://cp.city-mind.com](https://cp.city-mind.com/ "cp.city-mind.com")**.
If your registration was successful, then you can use this integration.

[![Self signup](https://user-images.githubusercontent.com/255973/88737784-c536be00-d141-11ea-819c-2199816e3511.png "https://cp.city-mind.com/")](https://cp.city-mind.com/)

Registration may not succeed for one of the following reasons:

- Your home water meters is not made by the brand "ARAD", as shown in the image above.
- Your water utility company does not allow residents access to the ["Read Your Meter"](https://cp.city-mind.com/ "https://cp.city-mind.com/") service (website cp.city-mind.com) that is offered by Arad Technologies.

Here is an outdated map showing water utilities companies in Israel that use Arad's water meters:

![map](https://user-images.githubusercontent.com/255973/87733202-c4b03600-c7d7-11ea-9c8c-7aff8c1f9e81.png "Supported water utilities")

Here is a partial list (may also be outdated) of supported water utilities and cities:

אפשרי רק ללקוח של אחד**מתאגידי המים** שמקבלים שרותי קריאת מונים אונליין מארד טכנולוגיות, לדוגמא:

- מיתב: פתח תקווה, אלעד
- מי-נעם: נצרת עילית, עפולה, מגדל העמק
- פלג הגליל: צפת ועוד
- מעיינות הדרום: דימונה, ערד, ירוחם, ומצפה רמון
- מי-מודיעין
- מי ציונה: נס ציונה, מזכרת בתיה, קריית עקרון
- מי התנור: קריית שמונה, מטולה, קצרין
- מי רקת טבריה
- מי עכו
- מעיינות זיו: מעלות
- מעיינות העמקים: יוקנעם, זכרון יעקב
- מניב ראשון: ראשון לציון
- יובלים בשומרון

---

## Installation

Make sure you have signed up at [https://cp.city-mind.com](https://cp.city-mind.com/ "cp.city-mind.com") as mentioned above, and have a working username/password.  The username is usually your email address.

It is recommended to install using HACS, but it is also easy to install manually

### Install using HACS (Recommended)

Add this repository to HACS as a custom repository.
 After few seconds (be patient), the option to install this integration
 will show up.  Just add it.

After adding with HACS go to Integrations UI (under Configuration page) and add the "Citymind-water-meter" integration.

### Install Manually

Copy the `/custom_components/citymind_water_meter` folder from this repository to your `<config_dir>/custom_components/` folder.
Restart Home Assistant.

After adding the custom component go to Integrations UI (under Configuration page), click "+" and add the "Citymind-water-meter" integration.

---

## Configuration

Configure using the "Integrations" UI.  Just fill in your username/password as mentioned above.

### Breaking Changes

When upgrading from v0.x.x to v1.0, please remove any use of "citymind_water_meter" platform from `configuration.yaml`.
Configuration is now only available via the UI.

---

## Example of a History Chart

Below is a history graph of a 24 hours meter readings.

Notice that the system only shows usage in "steps" of 100 liters.
That's the provided resolution by City-Mind service:

![Water Meter Reading Chart](https://user-images.githubusercontent.com/255973/87365060-eada9980-c57d-11ea-915a-0c1da95c2d4f.png "Water Meter Reading")

### Example of Lovelace Charts

For this example you would need to have the [Lovelace Mini Graph Card](https://github.com/kalkih/mini-graph-card "mini-graph-card GitHub repository") installed.
It is a highly recommended UI feature.
Install it using HACS.

In the UI, add the following card to see a 24-hours, and a 7-days charts:

```yaml
type: entities
title: Water Meter
entities:
  - type: 'custom:mini-graph-card'
    name: 24 Hours Water Meter
    entities:
      - entity: sensor.water_meter_XXXXXXXXX_last_reading
        name: Water Meter
    points_per_hour: 12
    smoothing: false
    show:
      labels: true
  - type: 'custom:mini-graph-card'
    name: 7 Days Water Consumption
    hours_to_show: 168
    group_by: date
    aggregate_func: delta
    entities:
      - sensor.water_meter_XXXXXXXXX_last_reading
    show:
      graph: bar
      state: false
  - type: weblink
    url: 'https://cp.city-mind.com/Default.aspx'
```

<img src =
  "https://user-images.githubusercontent.com/255973/95665125-f70ecc80-0b55-11eb-887f-edb3e1463051.png"
  height="459" width="367"
  alt="Sample charts using mini-graph-card">

## Why water meters in Israel have the 100-Liter tics? (Only in Israel)

Almost all water meters in Israel have the minimum resolution that is no less than 100 liters.

The reason for that is religious.
It allows normal use of water during a Saturday to not always triggers an electric pulse.
You can find all kosher water meters reasoning [in this article](https://www.zomet.org.il/?CategoryID=198&ArticleID=697#_Toc334393456).

Unfortunately, the 100 liter limitation in Israel reduces the water meter capabilities to identify water leaks.

*Glatt Kosher water meters* can support fine metering resolution because they have automatic timers that shut the meter down completely during Saturdays.

---

## Credits

This project was inspired by the [Read Your Meter](https://github.com/eyalcha/read_your_meter "Read Your Meter")
project, made by my neighbor [eyalcha](https://github.com/eyalcha/).

I created this alternative project because I wanted it lighter, quicker, and easy to
setup.  Mostly, I wanted to avoid the manual installation of a Selenium docker on Hass.io.

Kudos to [Elad Bar](https://github.com/elad-bar/) for his help, a wonderful code contribution, and refactoring.

[![Israel](https://raw.githubusercontent.com/hjnilsson/country-flags/master/png250px/il.png "Water Meter by Arad Technologies")](https://arad.co.il/products/residential/ "Israel Flag")
