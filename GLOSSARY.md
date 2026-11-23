# calchimi

A single-player management (tycoon) browser game: the player grows in knowledge and reputation as an alchemist, pharmacist, psychologist and doctor serving a town.

## Language

### Player

**PC**:
The player character: the one avatar the player moves and acts through in the town.
_Avoid_: Avatar, hero, player (when meaning the character)

### Specialities

**Speciality**:
One of the four fields the PC progresses in: Alchemy, Pharmacy, Psychology, Medicine. Each has its own Knowledge and Reputation.
_Avoid_: Class, job, profession, skill tree

**Alchemy**:
The Speciality of transforming matter by real chemical processes, plus a few deliberate fun exceptions (e.g. lead to gold) unlocked at high Knowledge.
_Avoid_: Chemistry (as a separate Speciality)

**Pharmacy**:
The Speciality of preparing and dispensing remedies.
_Avoid_: Apothecary (as a Speciality name)

**Psychology**:
The Speciality of treating patients' mental and emotional problems.
_Avoid_: Psychiatry (as a Speciality name)

**Medicine**:
The Speciality of diagnosing and treating patients' physical problems, as a general practitioner.
_Avoid_: General practice, GP (as a Speciality name)

**Knowledge**:
How much the PC has learned in one Speciality; gates what they can do in it.
_Avoid_: XP, level, skill

**Reputation**:
How much the town trusts the PC in one Speciality; drives which patients come.
_Avoid_: Fame, popularity

**Rank title**:
A flavour name for a stage of progress within a Speciality (e.g. apothecary, pharmacist; chemist, alchemist).
_Avoid_: Using a Rank title as the Speciality name

### Places

**Plot**:
The PC's property: the walkable area holding the Garden and the house with the Laboratory and the Consultation room.
_Avoid_: Map, level, base

**Garden**:
The place on the Plot where the PC grows plants for Resources.

**Bed**:
A fixed patch in the Garden where one plant grows.
_Avoid_: Plot (the whole property), field, slot, tile

**Seed**:
What the PC buys in the Shop and plants in a Bed to grow one kind of plant.
_Avoid_: Sapling, cutting

**Mine**:
A place where the PC extracts mineral Resources.
_Avoid_: Cave (as a separate place)

**Laboratory**:
The room where the PC turns Resources into Remedies using Setups.
_Avoid_: Lab (in player-facing text), workshop

**Setup**:
A piece of Laboratory equipment the PC buys in the Shop and installs at its spot to perform one kind of Process (e.g. a still for distillation).
_Avoid_: Machine, station, workbench

**Consultation room**:
The room where the PC receives Patients, diagnoses them and dispenses Remedies.
_Avoid_: Office, clinic

**Couch**:
The piece of furniture in the house where the PC Sleeps.
_Avoid_: Bed (a patch in the Garden)

### Production

**Resource**:
A material that is not a Remedy: gathered from the Garden or the Mine, or made from other Resources by a Process (e.g. dried peppermint leaf).
_Avoid_: Ingredient, material, item

**Process**:
A real-world transformation (maceration, distillation, extraction…) performed at a Setup that turns Resources into a Remedy or another Resource.
_Avoid_: Recipe, craft

**Remedy**:
A product of the Laboratory dispensed to a Patient to treat an Ailment.
_Avoid_: Medicine (clashes with the Speciality), potion, drug

**Batch**:
One run of a Process at a Setup, turning a chosen quantity of Resources into its output over a fixed time.
_Avoid_: Job, craft, order

**Stock**:
Everything the PC holds: Resources, Remedies, Seeds, and Setups not yet installed.
_Avoid_: Inventory, storage, bag

### Patients

**Patient**:
A townsperson who comes to the Consultation room with an Ailment, for an Appointment or as an Emergency, and pays when treated.
_Avoid_: Customer, client, NPC

**Ailment**:
The real-world health problem a Patient comes with, shown through symptoms.
_Avoid_: Disease, illness, condition, problem

**Opening hours**:
The whole-hour periods of each weekday, set by the player, during which Patients can book Appointments.
_Avoid_: Schedule, office hours, availability

**Open to the public**:
Whether the PC takes new Appointments and Emergencies at all; closed at the start.
_Avoid_: Open (alone, meaning one hour of the Opening hours)

**Appointment**:
One hour of the Opening hours booked by one Patient.
_Avoid_: Booking, reservation, slot

**Emergency**:
A Patient who arrives without an Appointment, at any hour.
_Avoid_: Walk-in, urgent case

### Economy

**Coin**:
The single currency: Patients pay in Coins, and the PC spends them in the Shop.
_Avoid_: Gold (reserved for the Alchemy material), money, credits

**Order**:
One paid cart in the Shop, delivered as one Parcel.
_Avoid_: Purchase, cart (before checkout)

**Parcel**:
The box an Order arrives in at the house door; unpacking it puts its contents into Stock.
_Avoid_: Package, delivery, box

### Time

**Game time**:
The Plot's own clock and calendar: a game day passes in one real hour, and time stands still while the game is not on screen.
_Avoid_: Real time, wall clock (for the in-game time)

**Sleep**:
Skipping game time on the Couch, until a chosen time or the next event.
_Avoid_: Rest, skip, fast-forward

### Smartphone

**Smartphone**:
The phone the PC carries in a transparent holster at the belt, holding the apps and Notifications.
_Avoid_: Mobile, cell phone, device

**Notification**:
A message on the Smartphone about an Appointment, an Emergency or a Parcel; unread until the player opens it.
_Avoid_: Alert, message

**Agenda**:
The Smartphone app listing past and upcoming Appointments and Emergencies.
_Avoid_: Calendar (the one on the wall), planner, schedule

**Shop**:
The Smartphone app where the PC buys Seeds and Setups with Coins.
_Avoid_: Store, market, merchant

**Trash**:
The Smartphone app holding removed Notifications and Agenda events until they are erased for good.
_Avoid_: Bin, recycle bin
