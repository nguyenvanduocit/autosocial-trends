---
source: hackernews
title: Janet Jackson had the power to crash laptop computers (2022)
url: https://devblogs.microsoft.com/oldnewthing/20220816-00/?p=106994
date: 2025-12-28
---

Skip to main content

[![](https://uhf.microsoft.com/images/microsoft/RE1Mu3b.png)
Microsoft](https://www.microsoft.com)

Dev Blogs

[Dev Blogs](https://devblogs.microsoft.com)

Dev Blogs

* [Home](https://devblogs.microsoft.com)
* Developer
  + [Microsoft for Developers](https://developer.microsoft.com/blog)
  + [Visual Studio](https://devblogs.microsoft.com/visualstudio/)
  + [Visual Studio Code](https://devblogs.microsoft.com/vscode-blog)
  + [Develop from the cloud](https://devblogs.microsoft.com/develop-from-the-cloud/)
  + [All things Azure](https://devblogs.microsoft.com/all-things-azure/)
  + [Xcode](https://devblogs.microsoft.com/xcode/)
  + [DevOps](https://devblogs.microsoft.com/devops/)
  + [Windows Developer](https://blogs.windows.com/windowsdeveloper/)
  + [ISE Developer](https://devblogs.microsoft.com/ise/)
  + [Azure SDK](https://devblogs.microsoft.com/azure-sdk/)
  + [Command Line](https://devblogs.microsoft.com/commandline/)
  + [Aspire](https://devblogs.microsoft.com/aspire/)
* Technology
  + [DirectX](https://devblogs.microsoft.com/directx/)
  + [Semantic Kernel](https://devblogs.microsoft.com/semantic-kernel/)
* Languages
  + [C++](https://devblogs.microsoft.com/cppblog/)
  + [C#](https://devblogs.microsoft.com/dotnet/category/csharp/)
  + [F#](https://devblogs.microsoft.com/dotnet/category/fsharp/)
  + [TypeScript](https://devblogs.microsoft.com/typescript/)
  + [PowerShell Team](https://devblogs.microsoft.com/powershell/)
  + [Python](https://devblogs.microsoft.com/python/)
  + [Java](https://devblogs.microsoft.com/java/)
  + [Java Blog in Chinese](https://devblogs.microsoft.com/java-ch/)
  + [Go](https://devblogs.microsoft.com/go/)
* .NET
  + [All .NET posts](https://devblogs.microsoft.com/dotnet/%20)
  + [.NET Aspire](https://devblogs.microsoft.com/dotnet/category/dotnet-aspire/)
  + [.NET MAUI](https://devblogs.microsoft.com/dotnet/category/maui/)
  + [AI](https://devblogs.microsoft.com/dotnet/category/ai/)
  + [ASP.NET Core](https://devblogs.microsoft.com/dotnet/category/aspnetcore/)
  + [Blazor](https://devblogs.microsoft.com/dotnet/category/blazor/)
  + [Entity Framework](https://devblogs.microsoft.com/dotnet/category/entity-framework/)
  + [NuGet](https://devblogs.microsoft.com/dotnet/category/nuget/)
  + [Servicing](https://devblogs.microsoft.com/dotnet/category/maintenance-and-updates/)
  + [.NET Blog in Chinese](https://devblogs.microsoft.com/dotnet-ch/)
* Platform Development
  + [#ifdef Windows](https://devblogs.microsoft.com/ifdef-windows/)
  + [Microsoft Foundry](https://devblogs.microsoft.com/foundry/)
  + [Azure Government](https://devblogs.microsoft.com/azuregov/)
  + [Azure VM Runtime Team](https://devblogs.microsoft.com/azure-vm-runtime/)
  + [Bing Dev Center](https://blogs.bing.com/Developers-Blog/)
  + [Microsoft Edge Dev](http://blogs.windows.com/msedgedev/)
  + [Microsoft Azure](http://azure.microsoft.com/blog/)
  + [Microsoft 365 Developer](https://devblogs.microsoft.com/microsoft365dev/)
  + [Microsoft Entra Identity Developer](https://devblogs.microsoft.com/identity/)
  + [Old New Thing](https://devblogs.microsoft.com/oldnewthing/)
  + [Power Platform](https://devblogs.microsoft.com/powerplatform/)
* Data Development
  + [Azure Cosmos DB](https://devblogs.microsoft.com/cosmosdb/)
  + [Azure Data Studio](https://cloudblogs.microsoft.com/sqlserver/?product=azure-data-studio)
  + [Azure SQL](https://devblogs.microsoft.com/azure-sql/)
  + [OData](https://devblogs.microsoft.com/odata/)
  + [Revolutions R](http://blog.revolutionanalytics.com/)
  + [Unified Data Model (IDEAs)](https://devblogs.microsoft.com/udm/)
  + [Microsoft Entra PowerShell](https://devblogs.microsoft.com/entrapowershell/)
* More

Search
Search

* No results

Cancel

* [Dev Blogs](https://devblogs.microsoft.com/)
* [The Old New Thing](https://devblogs.microsoft.com/oldnewthing/)
* Janet Jackson had the power to crash laptop computers

August 16th, 2022

![compelling](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/emojis/compelling.svg)![heart](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/emojis/heart.svg)30 reactions

# Janet Jackson had the power to crash laptop computers

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

[Raymond Chen](https://devblogs.microsoft.com/oldnewthing/author/oldnewthing)

##

Show more

A colleague of mine shared a story from Windows XP product support. A major computer manufacturer discovered that playing the music video for Janet Jackson’s “[Rhythm Nation](https://en.wikipedia.org/wiki/Rhythm_Nation)” would crash certain models of laptops. I would not have wanted to be in the laboratory that they must have set up to investigate this problem. Not an artistic judgement.

One discovery during the investigation is that playing the music video also crashed some of their competitors’ laptops.

And then they discovered something extremely weird: Playing the music video on one laptop caused a laptop sitting nearby to crash, even though that other laptop wasn’t playing the video!

What’s going on?

It turns out that the song contained one of the natural resonant frequencies for the model of 5400 rpm laptop hard drives that they and other manufacturers used.

The manufacturer worked around the problem by adding a custom filter in the audio pipeline that detected and removed the offending frequencies during audio playback.

And I’m sure they put a digital version of a “Do not remove” sticker on that audio filter. (Though I’m worried that in the many years since the workaround was added, nobody remembers why it’s there. Hopefully, their laptops are not still carrying this audio filter to protect against damage to a model of hard drive they are no longer using.)

And of course, no story about natural resonant frequencies can pass without a reference to  [the collapse of the Tacoma Narrows Bridge in 1940](https://www.history.com/this-day-in-history/tacoma-narrows-bridge-collapses).¹

**Related**:  [Shouting in the Datacenter](https://www.youtube.com/watch?v=tDacjrSCeq4).

**Bonus chatter**:  [Video version of this story](https://twitter.com/WindowsDocs/status/1558114944738103297) and a  [Twitter poll](https://twitter.com/WindowsDocs/status/1558115433449852929).

Also, [Larry Osterman](https://twitter.com/osterman) had a similar experience with  [a specific game that crashed a prototype PC](https://twitter.com/osterman/status/1558676353196494850).

**Follow-up**:  [Janet Jackson had the power to crash laptop computers, follow-up](https://devblogs.microsoft.com/oldnewthing/20220920-00/?p=107201).

¹ **Follow-up 2**: Yes, I know that the Tacoma Narrows Bridge collapse was not the result of resonance, but I felt I had to drop the reference to forestall the “You forgot to mention the Tacoma Narrows Bridge!” comments.

[30](https://devblogs.microsoft.com/oldnewthing/wp-login.php?redirect_to=https%3A%2F%2Fdevblogs.microsoft.com%2Foldnewthing%2F20220816-00%2F%3Fp%3D106994 "Sign in to react")

[24](#comments "Go to comments section")

5

* [![Facebook](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/social-icons/facebook.svg)
  Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https://devblogs.microsoft.com/oldnewthing/20220816-00/?p=106994 "Share on Facebook")
* [Share on X](https://twitter.com/intent/tweet?url=https://devblogs.microsoft.com/oldnewthing/20220816-00/?p=106994&text=Janet Jackson had the power to crash laptop computers "Share on X")
* [![LinkedIn](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/social-icons/linkedin.svg)
  Share on Linkedin](https://www.linkedin.com/shareArticle?mini=true&url=https://devblogs.microsoft.com/oldnewthing/20220816-00/?p=106994 "Share on LinkedIn")

Category

[Old New Thing](https://devblogs.microsoft.com/oldnewthing/category/oldnewthing)

Topics

[History](https://devblogs.microsoft.com/oldnewthing/tag/history)

Share

## Author

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

[Raymond Chen](https://devblogs.microsoft.com/oldnewthing/author/oldnewthing)

Raymond has been involved in the evolution of Windows for more than 30 years. In 2003, he began a Web site known as The Old New Thing which has grown in popularity far beyond his wildest imagination, a development which still gives him the heebie-jeebies. The Web site spawned a book, coincidentally also titled The Old New Thing (Addison Wesley 2007). He occasionally appears on the Windows Dev Docs Twitter account to tell stories which convey no useful information.

## 24 comments

Discussion is closed. [Login to edit/delete existing comments.](https://devblogs.microsoft.com/oldnewthing/wp-login.php?redirect_to=https%3A%2F%2Fdevblogs.microsoft.com%2Foldnewthing%2F20220816-00%2F%3Fp%3D106994%23comments "Login to edit/delete existing comments.")

[Code of Conduct](https://aka.ms/msftqacodeconduct)

Sort by :

Newest

Newest
Popular
Oldest

* ![](data:image/png;base64...)

  Alexis Talbot

  August 29, 2022
   1

  Collapse this comment

  Copy link

  This kind of issues are still very much present in today's storage servers. I work for a company who specializes in Acoustic simulation and we help some customers to avoid having resonances create reading / writing problems in hard drives. The source of the noise / vibrations is often the cooling fan as companies are trying to compact more and more hard drives in the same volume, putting more constraint on the power for the fan to ensure thermal performances. You can read more here if interested, it's a short article explaining how Meta deals with this kind of issues:...

  Read more

  This kind of issues are still very much present in today’s storage servers. I work for a company who specializes in Acoustic simulation and we help some customers to avoid having resonances create reading / writing problems in hard drives. The source of the noise / vibrations is often the cooling fan as companies are trying to compact more and more hard drives in the same volume, putting more constraint on the power for the fan to ensure thermal performances. You can read more here if interested, it’s a short article explaining how Meta deals with this kind of issues: <https://www.mscsoftware.com/sites/default/files/optimising-storage-server-chassis-design-with-aeroacoustic-simulations.pdf>

  Read less
* ![](data:image/png;base64...)

  Earlisa Norman

  August 23, 2022
   0

  Collapse this comment

  Copy link

  Dang it! I was gonna play it on my phone at work & shut the office down but it’s been resolved! 🤬😠😡
* ![](data:image/png;base64...)

  Owen Rubin

  August 22, 2022
   1

  Collapse this comment

  Copy link

  I guess they were lucky in the recording studio that none of their machines had such drives. That would be some support call: “Every time we try and record this song all our machines crash!” I can just imagine that support call to Microsoft.
* ![](data:image/png;base64...)

  Gonch Gardner

  August 19, 2022
   0

  Collapse this comment

  Copy link

  Yes and a colleague once told me gullible had been removed from the dictionary.
* ![](data:image/png;base64...)

  Petri Oksanen

  August 19, 2022
   1

  Collapse this comment

  Copy link

  There is also the story of the resonant frequency of chicken skulls from the old Borland Turbo C++ documentation:
  [https://everything2.com/title/7+hertz+-+the+resonant+frequency+of+a+chicken%2527s+skull](https://everything2.com/title/7%2Bhertz%2B-%2Bthe%2Bresonant%2Bfrequency%2Bof%2Ba%2Bchicken%2527s%2Bskull)

  Quote:
  **True story: 7 Hz is the resonant
  frequency of a chicken’s skull cavity.
  This was determined empirically in
  Australia, where a new factory
  generating 7-Hz tones was located too
  close to a chicken ranch: When the
  factory started up, all the chickens
  died.**
* ![](data:image/png;base64...)

  unforseen consequencer

  August 18, 2022
   0

  Collapse this comment

  Copy link

  Why did they ruined the audio instead of making proper isolation for hard drives though? Filtering out frequencies from the audio without user’s consent because of crappy hardware is a very ugly workaround. And laptops can still be crashed by malicious actor using other audio devices.
* ![](data:image/png;base64...)

  Jasper Nuyens

  August 18, 2022
   0

  Collapse this comment

  Copy link

  Original message posted on the 1st of April 1997? 😉
* ![](data:image/png;base64...)

  microsoftonline-com

  August 18, 2022
  · Edited
   0

  Collapse this comment

  Copy link

  Mr. Chen, could you please clarify if your colleague in Windows XP product support witnessed this or if this is one of those urban legends that happened to "a friend of a friend". Also, I'm wondering how you feel about CVE 2022-38392 which is entirely based on this blog entry. While it is hilarious to think that Janet Jackson's Rhythm Nation is a Denial of Service attack, it seems almost preposterous that a laptop could be that shoddily built. Did they not have proper damping of vibrations?

  That said, I did analyze the frequency spectrum using Audacity and the most prominent...

  Read more

  Mr. Chen, could you please clarify if your colleague in Windows XP product support witnessed this or if this is one of those urban legends that happened to “a friend of a friend”. Also, I’m wondering how you feel about [CVE 2022-38392](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-38392) which is entirely based on this blog entry. While it is hilarious to think that Janet Jackson’s Rhythm Nation is a Denial of Service attack, it seems almost preposterous that a laptop could be that shoddily built. Did they not have proper damping of vibrations?

  That said, I did analyze the frequency spectrum using Audacity and the most prominent peak is at 84⅛ Hz, which is a smidge higher than E2 on a piano. As it happens, 5400 Hz is a smidge higher than E6 (you’ll need an extended 108-key keyboard for that one). Being the same note, six octaves apart, I have to admit that if any song is going to cause problems due to resonant frequencies, Rhythm Nation would be it.

  This should be easy to test with a signal generator. If one has sox installed, you could simply run

  ```
  play -n synth sin E2
  ```

  and see what hard drives start logging ATA retries. Or, to be more accurate, one could use

  ```
  play -n synth sin 84.375
  ```

  which is 5400÷2⁶, while E2 is 82.40689 Hz.

  Read less

  + ![](data:image/png;base64...)

    Jonathan Brochu

    August 23, 2022
    · Edited
     1

    Collapse this comment

    Copy link

    Correct me if I'm wrong, but if I recall my physics lessons correctly, resonance has little to do with the speed of rotation of an object\*\*, and more about the natural frequency of vibration for that object, which is linked to (1) its length, and (2) the speed of a wave traveling through that object (the latter affected by object composition, e.g. aluminum, steel, etc.; its stiffness/thickness, i.e. the stiffer it is the faster it'll travel; and its mass, i.e. the heavier the slower the traveling wave will be). Actually, the length would be a pretty major factor since it...

    Read more

    Correct me if I’m wrong, but if I recall my physics lessons correctly, resonance has little to do with the speed of rotation of an object\*\*, and more about the natural frequency of vibration for that object, which is linked to (1) its length, and (2) the speed of a wave traveling through that object (the latter affected by object composition, e.g. aluminum, steel, etc.; its stiffness/thickness, i.e. the stiffer it is the faster it’ll travel; and its mass, i.e. the heavier the slower the traveling wave will be). Actually, the length would be a pretty major factor since it would dictate which frequency an object is “vulnerable” to by resonance; alter its length and the “attack” frequency no longer has any effect. Make any sense?

    So, for a laptop HDD, for the length we would start with 2.5″ (so says <http://209.68.14.80/ref/hdd/op/mediaSize-c.html>) \*minus\* the diameter of the platter’s machined inner hole, with that measure then divided by 2 (i.e. the platter being a 2D annulus, the width of one of the two “bands” when you cut the annulus in half through its center).

    But I wouldn’t have a clue as to the natural frequency of a single 2.5″ HDD platter; either it needs to be determined experimentally, or else there may be tables for common materials (with given density) by their thickness (or engineering software stacks/packages capable of computing them).

    So, it’s probably more complicated than 5400RPM = 90 Hz.

    [\*\*wouldn’t angular momentum make platters \*less\* susceptible to vibration *perpendicular to their surface, i.e. along the normal*?]

    Read less

    - ![](data:image/png;base64...)

      Ron Parker![Microsoft employee](https://devblogs.microsoft.com/oldnewthing/wp-content/plugins/devblogs-comments-evo/admin/images/MicrosoftLogo_50x50.png "Microsoft employee")

      August 24, 2022
       2

      Collapse this comment

      Copy link

      I think it's more complicated than that, even. Resonant modes of a flat disc with a hole in it would be determined by a set of partial differential equations that I don't even want to think about, but the solution would also depend on how the disc is supported - that is, if you took the platter out of the drive and hung it on a string, it would have different resonance than it does when the entire inner circumference is held fixed inside the drive.

      It doesn't have to be the platter, though - most of the drives I've torn...

      Read more

      I think it’s more complicated than that, even. Resonant modes of a flat disc with a hole in it would be determined by a set of partial differential equations that I don’t even want to think about, but the solution would also depend on how the disc is supported – that is, if you took the platter out of the drive and hung it on a string, it would have different resonance than it does when the entire inner circumference is held fixed inside the drive.

      It doesn’t have to be the platter, though – most of the drives I’ve torn apart had the heads on the end of long (relatively speaking) flat strips of spring steel, supported only at the end away from the head, so any resonance in them would have had its maximum amplitude at the head end, right where you really don’t want it.

      Read less
  + ![](data:image/png;base64...)

    Akash Bagh

    August 22, 2022
     1

    Collapse this comment

    Copy link

    > As it happens, 5400 Hz is a smidge higher than E6.

    @microsoftonline-com : 5400 rpm is 90 Hz. How is 5400 Hz relevant? 🙂
  + ![](data:image/png;base64...)

    MGetz

    August 19, 2022
     0

    Collapse this comment

    Copy link

    > Did they not have proper damping of vibrations?

    Many consumer laptops even as recently as 2013 didn’t bother with any sort of dampening because that would increase cost. Netbooks (what anecdotes from other commenters seem to indicate was the likely place this occurred) would definitely not have had any due to both size and cost reasons. 10¢ of rubber dampening matters a lot when competing in the sub $300 category sadly. When you build to a price you get what you pay for honestly.
  + ![](data:image/png;base64...)

    Akash Bagh

    August 19, 2022
     0

    Collapse this comment

    Copy link

    Thanks for asking this question, this sounds like one of those stories that are too good to be true. If confirmed, it would make an excellent example while teaching the physics of resonances. So any sort of further detail would be very much appreciated.
  + ![](data:image/png;base64...)

    Brendan Bonner

    August 19, 2022
     1

    Collapse this comment

    Copy link

    The story seems to centre around the video, which contains around 20 seconds of just above sub-bass at around 50Hz to 100Hz with a couple of strong 82.4Hz amongst it. Surprised it didn’t happen more (if the drives were built to not dampen this)!
  + ![](data:image/png;base64...)

    [Raymond Chen![Microsoft employee](https://devblogs.microsoft.com/oldnewthing/wp-content/plugins/devblogs-comments-evo/admin/images/MicrosoftLogo_50x50.png "Microsoft employee") Author](https://devblogs.microsoft.com/oldnewthing/author/oldnewthing "Raymond Chen")

    August 18, 2022
    · Edited
     1

    Collapse this comment

    Copy link

    My colleague claims to have been part of the team that investigated the problem. I accept his word for it. (I also think the CVE is somebody playing a joke.)
* ![](data:image/png;base64...)

  Brian Boorman

  August 18, 2022
  · Edited
   1

  Collapse this comment

  Copy link

  This blog post became the basis for a whole news story on Ars Technica today:
  [Old laptop hard drives will allegedly crash when exposed to Janet Jackson music](https://arstechnica.com/gadgets/2022/08/janet-jacksons-rhythm-nation-is-officially-a-security-threat-for-some-old-laptops/)

  At least this time Raymond is described as a Software Engineer and not Microsoft Executive.
  [DailyTech identified me as a Microsoft executive](https://devblogs.microsoft.com/oldnewthing/20140909-00/?p=44123)
* ![](data:image/png;base64...)

  Jan Tångring

  August 18, 2022
   0

  Collapse this comment

  Copy link

  <https://genius.com/Douglas-hofstadter-contracrostipunctus-annotated>

  It is the Contracrostipunctus all over again!

Load more comments

## Read next

August 17, 2022

### [The AArch64 processor (aka arm64), part 16: Conditional execution](https://devblogs.microsoft.com/oldnewthing/20220817-00/?p=106998)

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

Raymond Chen

August 18, 2022

### [The AArch64 processor (aka arm64), part 17: Manipulating flags](https://devblogs.microsoft.com/oldnewthing/20220818-00/?p=107005)

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

Raymond Chen

## Stay informed

Get notified when new posts are published.

Email \*

Country/Region \*
Select...United StatesAfghanistanÅland IslandsAlbaniaAlgeriaAmerican SamoaAndorraAngolaAnguillaAntarcticaAntigua and BarbudaArgentinaArmeniaArubaAustraliaAustriaAzerbaijanBahamasBahrainBangladeshBarbadosBelarusBelgiumBelizeBeninBermudaBhutanBoliviaBonaireBosnia and HerzegovinaBotswanaBouvet IslandBrazilBritish Indian Ocean TerritoryBritish Virgin IslandsBruneiBulgariaBurkina FasoBurundiCabo VerdeCambodiaCameroonCanadaCayman IslandsCentral African RepublicChadChileChinaChristmas IslandCocos (Keeling) IslandsColombiaComorosCongoCongo (DRC)Cook IslandsCosta RicaCôte dIvoireCroatiaCuraçaoCyprusCzechiaDenmarkDjiboutiDominicaDominican RepublicEcuadorEgyptEl SalvadorEquatorial GuineaEritreaEstoniaEswatiniEthiopiaFalkland IslandsFaroe IslandsFijiFinlandFranceFrench GuianaFrench PolynesiaFrench Southern TerritoriesGabonGambiaGeorgiaGermanyGhanaGibraltarGreeceGreenlandGrenadaGuadeloupeGuamGuatemalaGuernseyGuineaGuinea-BissauGuyanaHaitiHeard Island and McDonald IslandsHondurasHong Kong SARHungaryIcelandIndiaIndonesiaIraqIrelandIsle of ManIsraelItalyJamaicaJan MayenJapanJerseyJordanKazakhstanKenyaKiribatiKoreaKosovoKuwaitKyrgyzstanLaosLatviaLebanonLesothoLiberiaLibyaLiechtensteinLithuaniaLuxembourgMacau SARMadagascarMalawiMalaysiaMaldivesMaliMaltaMarshall IslandsMartiniqueMauritaniaMauritiusMayotteMexicoMicronesiaMoldovaMonacoMongoliaMontenegroMontserratMoroccoMozambiqueMyanmarNamibiaNauruNepalNetherlandsNew CaledoniaNew ZealandNicaraguaNigerNigeriaNiueNorfolk IslandNorth MacedoniaNorthern Mariana IslandsNorwayOmanPakistanPalauPalestinian AuthorityPanamaPapua New GuineaParaguayPeruPhilippinesPitcairn IslandsPolandPortugalPuerto RicoQatarRéunionRomaniaRwandaSabaSaint BarthélemySaint Kitts and NevisSaint LuciaSaint MartinSaint Pierre and MiquelonSaint Vincent and the GrenadinesSamoaSan MarinoSão Tomé and PríncipeSaudi ArabiaSenegalSerbiaSeychellesSierra LeoneSingaporeSint EustatiusSint MaartenSlovakiaSloveniaSolomon IslandsSomaliaSouth AfricaSouth Georgia and South Sandwich IslandsSouth SudanSpainSri LankaSt HelenaAscensionTristan da CunhaSurinameSvalbardSwedenSwitzerlandTaiwanTajikistanTanzaniaThailandTimor-LesteTogoTokelauTongaTrinidad and TobagoTunisiaTurkeyTurkmenistanTurks and Caicos IslandsTuvaluU.S. Outlying IslandsU.S. Virgin IslandsUgandaUkraineUnited Arab EmiratesUnited KingdomUruguayUzbekistanVanuatuVatican CityVenezuelaVietnamWallis and FutunaYemenZambiaZimbabwe

I would like to receive the The Old New Thing Newsletter. [Privacy Statement.](https://go.microsoft.com/fwlink/?LinkId=521839)

Subscribe

Follow this blog

[![youtube](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/social-icons/youtube.svg)](https://www.youtube.com/playlist?list=PLlrxD0HtieHge3_8Dm48C0Ns61I6bHThc "youtube")

Are you sure you wish to delete this
comment?

OK
Cancel

[Sign in](https://devblogs.microsoft.com/oldnewthing/wp-login.php?redirect_to=https%3A%2F%2Fdevblogs.microsoft.com%2Foldnewthing%2F20220816-00%2F%3Fp%3D106994)

Theme

##### Code Block

×

Paste your code snippet

Ok
Cancel

What's new

* [Surface Pro](https://www.microsoft.com/surface/devices/surface-pro)
* [Surface Laptop](https://www.microsoft.com/surface/devices/surface-laptop)
* [Surface Laptop Studio 2](https://www.microsoft.com/en-us/d/Surface-Laptop-Studio-2/8rqr54krf1dz)
* [Copilot for organizations](https://www.microsoft.com/en-us/microsoft-copilot/organizations?icid=DSM_Footer_CopilotOrganizations)
* [Copilot for personal use](https://www.microsoft.com/en-us/microsoft-copilot/for-individuals?form=MY02PT&OCID=GE_web_Copilot_Free_868g3t5nj)
* [AI in Windows](https://www.microsoft.com/en-us/windows/ai-features?icid=DSM_Footer_WhatsNew_AIinWindows)
* [Explore Microsoft products](https://www.microsoft.com/en-us/microsoft-products-and-apps)
* [Windows 11 apps](https://www.microsoft.com/en-us/windows/apps-for-windows?icid=DSM_Footer_WhatsNew_Windows11apps)

Microsoft Store

* [Account profile](https://account.microsoft.com/)
* [Download Center](https://www.microsoft.com/en-us/download)
* [Microsoft Store support](https://go.microsoft.com/fwlink/?linkid=2139749)
* [Returns](https://www.microsoft.com/en-us/store/b/returns)
* [Order tracking](https://www.microsoft.com/en-us/store/b/order-tracking)
* [Certified Refurbished](https://www.microsoft.com/en-us/store/b/certified-refurbished-products)
* [Microsoft Store Promise](https://www.microsoft.com/en-us/store/b/why-microsoft-store?icid=footer_why-msft-store_7102020)
* [Flexible Payments](https://www.microsoft.com/en-us/store/b/payment-financing-options?icid=footer_financing_vcc)

Education

* [Microsoft in education](https://www.microsoft.com/en-us/education)
* [Devices for education](https://www.microsoft.com/en-us/education/devices/overview)
* [Microsoft Teams for Education](https://www.microsoft.com/en-us/education/products/teams)
* [Microsoft 365 Education](https://www.microsoft.com/en-us/education/products/microsoft-365)
* [How to buy for your school](https://www.microsoft.com/education/how-to-buy)
* [Educator training and development](https://education.microsoft.com/)
* [Deals for students and parents](https://www.microsoft.com/en-us/store/b/education)
* [AI for education](https://www.microsoft.com/en-us/education/ai-in-education)

Business

* [Microsoft Cloud](https://www.microsoft.com/en-us/microsoft-cloud)
* [Microsoft Security](https://www.microsoft.com/en-us/security)
* [Dynamics 365](https://www.microsoft.com/en-us/dynamics-365)
* [Microsoft 365](https://www.microsoft.com/en-us/microsoft-365/business)
* [Microsoft Power Platform](https://www.microsoft.com/en-us/power-platform)
* [Microsoft Teams](https://www.microsoft.com/en-us/microsoft-teams/group-chat-software)
* [Microsoft 365 Copilot](https://www.microsoft.com/en-us/microsoft-365-copilot?icid=DSM_Footer_Microsoft365Copilot)
* [Small Business](https://www.microsoft.com/en-us/store/b/business?icid=CNavBusinessStore)

Developer & IT

* [Azure](https://azure.microsoft.com/en-us/)
* [Microsoft Developer](https://developer.microsoft.com/en-us/)
* [Microsoft Learn](https://learn.microsoft.com/)
* [Support for AI marketplace apps](https://www.microsoft.com/software-development-companies/offers-benefits/isv-success?icid=DSM_Footer_SupportAIMarketplace&ocid=cmm3atxvn98)
* [Microsoft Tech Community](https://techcommunity.microsoft.com/)
* [Microsoft Marketplace](https://marketplace.microsoft.com?icid=DSM_Footer_Marketplace&ocid=cmm3atxvn98)
* [Marketplace Rewards](https://www.microsoft.com/software-development-companies/offers-benefits/marketplace-rewards?icid=DSM_Footer_MarketplaceRewards&ocid=cmm3atxvn98)
* [Visual Studio](https://visualstudio.microsoft.com/)

Company

* [Careers](https://careers.microsoft.com/)
* [About Microsoft](https://www.microsoft.com/about)
* [Company news](https://news.microsoft.com/source/?icid=DSM_Footer_Company_CompanyNews)
* [Privacy at Microsoft](https://www.microsoft.com/en-us/privacy?icid=DSM_Footer_Company_Privacy)
* [Investors](https://www.microsoft.com/investor/default.aspx)
* [Diversity and inclusion](https://www.microsoft.com/en-us/diversity/default?icid=DSM_Footer_Company_Diversity)
* [Accessibility](https://www.microsoft.com/en-us/accessibility)
* [Sustainability](https://www.microsoft.com/en-us/sustainability/)

[Your Privacy Choices Opt-Out Icon

Your Privacy Choices](https://aka.ms/yourcaliforniaprivacychoices)

[Your Privacy Choices Opt-Out Icon

Your Privacy Choices](https://aka.ms/yourcaliforniaprivacychoices)
[Consumer Health Privacy](https://go.microsoft.com/fwlink/?linkid=2259814)

* [Sitemap](https://www.microsoft.com/en-us/sitemap1.aspx)
* [Contact Microsoft](https://support.microsoft.com/contactus)
* [Privacy](https://go.microsoft.com/fwlink/?LinkId=521839)
* Manage cookies
* [Terms of use](https://go.microsoft.com/fwlink/?LinkID=206977)
* [Trademarks](https://go.microsoft.com/fwlink/?linkid=2196228)
* [Safety & eco](https://go.microsoft.com/fwlink/?linkid=2196227)
* [Recycling](https://www.microsoft.com/en-us/legal/compliance/recycling)
* [About our ads](https://choice.microsoft.com)
* © Microsoft 2025
