Currently in testing a MBC1 based flash cart with FRAM and AM29F080 ROM.

![Gameboy MBC1 Cart + FRAM](https://user-images.githubusercontent.com/65309612/214947262-9b0cdd31-971c-4be8-99af-f48ddeae7723.jpg)

![Gameboy MBC1 Cart + FRAM Final 2](https://user-images.githubusercontent.com/65309612/213887359-5e74cb24-b308-41f7-8a23-38a8cecf73e4.jpg)

**EEPROM AM29F080B**

I've found them available from Aliexpress very easily. I got them for just over £1 per chip. These are a 16-bit address and 8-bit data bus EEPROM. The write enable pin is brought to the audio pin on the Gameboy cartridge header. This is compatible with the GBxCart RW. The pin is held to 5v to keep it disabled during usage through a pullup resistor. You can buy 0603 spec 10k resistors or reuse the resistor on an original Nintendo doner PCB. I've not seen any need for this pull up resistor, but I can see it will cause a problem down the line somehow without it. 70 hours into Pokemon Yellow I wouldn't want the EEPROM to get corrupted and loose your save. I'm not sure if the audio pin is held low by the Gameboy, I think it is, so likely there isn't a need for it. You can omit it if you like. You'll use 500uA of current.

**SRAM FM18W08**

The SRAM isn't SRAM, its the modern FRAM that was made to replace battery backed SRAM in industrial PLC applications (among other things I'm sure). Once more Aliexpress has these in a small supply. These are the most expensive part of building this, apart from the donor MBC5 chip, depends on where you get the MBC5. The FRAM is available on eBay but the prices are egregious. You can get quality versions of the FRAM from reputable suppliers like Mouser and Digikey. The FM1808, FM18W08, FM1808B and FM18L08 all work in this desgin. FM1808 and the FM18L08 are out of production, so any you find will be old stock or poor copies. Bear in mind that the L version is a 3.3v part that is able to stand an extreme of 5v at its input, so I don't suggest using that if possible.

So in my tinkering with all this, I can suggest that you spend £12 per chip for a quality item. Aliexpress and eBay are full of either copies or factory rejected parts. You can find them for around £2 per chip and you get junk. In a batch of 5 from Aliexpress, none of them worked. From another batch, I went through 4 before finding a working one. In the end I went through 8 chips before getting one that works. So 9 chips x £2 is £18 for one working chip. Better off spending the £12 for one.

**Memory Controller MBC1**

This is the most expensive part of the cartridge. That does depend though. You'll need a heat gun to remove the chip from the board. It can be done with a soldering iron with gentle prising and patience. These are easily found in a lot of early GB games. There are great lists out there that can show you what hardware is in each game. A lot of the early games didn't use a memory controller or used and MBC2 instead with its own save RAM built in. A cost reducing measure from Nintendo. I'll update when I've sourced a bunch of chips and see what the average price is.

You can use ROMs up to 4MB with this controller but games rarely ever used that function. You loose a lot of game save capacity, so it made it a little pointless. You have a bigger game you'd think there would also be more data to save. That and likely game devs didn't want to pay for the more expensive ROM chips for every game sold.

**Switchable Design**

Adding FRAM to the circuit design requires minimal mods to be done. It requires a pull up resistor on pin 20 for stability. I've not seen any downside to not having that there yet. This is not completely true for some games on this MBC. Super Mario 6 Golden Coins for whatever reason is programmed differently and causes havoc with the save RAM. The thing with SRAM is that you can change the address on its inputs and the data output will change. Well that doesn't work with FRAM. Each adress change requires chip enable to be cycled before changing the address. The address is saved to the FRAM and the inputs ignored until the data is output, the address forgotten and chip enable cycled. So I suspect that the programmers took advantage of that feature when programming the game. As with some other games like Pokemon, the SRAM is used as work RAM and sprites are decompressed there. 

In the top corner is space to fit a 3 input OR gate to stobe the chip enable input with a clock pulse and also whenever normal chip enable requests are sent. Creating an artificial chip enable pulse. This is probably compatible with all MBC1 based games but I left a simple jumper system in place to allow the OR gate to not be fitted in the case where you may not wish to use the cart for games that need it.

For Normal usage: Leave unpopulated, R4 and U5. Place a link or a 0R resistor in R3 position.

For Mario games: Leave unpopulated R3. Populate U5 and R4.

I would recommend installing the OR gate regardless of if you plan on using games that require it. It seems to work just fine with it installed for all games I've so far tested. I've also had good results without using the OR gate with Mario 6 Golden Coins. They are quite expensive though, so you could also argue that you don't need to install it.

Changelog:

07/02/2023 Updated readme now that I've been able to test the OR gate on the PCB.

26/01/2023 V1.1: Rerouted some traces to tidy things up. Added missing trace to MBC1 reset pin. Changed the corner notch size to better fit into shells.

21/01/2023 V1.0: Missing MBC1 reset trace. Requires botch wire.
