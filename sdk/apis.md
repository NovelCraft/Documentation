# Common API Structure

All provided SDKs follows the same API structure, though they may differ in the way they are implemented and named. The following is a list of all the APIs that are available in the SDKs.

- Sdk

  - Sdk.Initialize(Config)

  - Sdk.GetAgent() -> Agent

  - Sdk.GetBlocks() -> BlockSource

  - Sdk.GetClient() -> Client

  - Sdk.GetEntities() -> EntitySource

  - Sdk.GetLogger() -> Logger

  - Sdk.GetLatency() -> Duration

  - Sdk.GetTick() -> int

  - Sdk.GetTicksPerSecond() -> float


- Agent

  - Agent.GetOrientation() -> Orientation

  - Agent.GetPosition() -> Position

  - Agent.GetTypeId() -> int

  - Agent.GetUniqueId() -> int

  - Agent.GetHealth() -> float

  - Agent.GetInventory() -> Inventory

  - Agent.GetMovement() -> MovementKind

  - Agent.SetMovement(Movement)

  - Agent.GetToken() -> string

  - Agent.Attack(InteractionKind)

  - Agent.Jump()

  - Agent.LookAt(Position)

  - Agent.Use(InteractionKind)


- Inventory

  - Inventory.GetItem(int) -> Item

  - Inventory.GetHorBarSize() -> int

  - Inventory.GetMainHandSlot() -> int

  - Inventory.GetSize() -> int

  - Inventory.Craft(ingredients: int[])

  - Inventory.DropItem(slot: int, count: int)

  - Inventory.MergeSlots(from: int, to: int)

  - Inventory.SwapSlots(slotA: int, slotB: int)


- Item

  - Item.GetCount() -> int

  - Item.GetTypeId() -> int


- BlockSource

  - GetBlock(Position) -> Block


- Block

  - GetPosition() -> Position

  - GetTypeId() -> int


- Client

  - Client.GetBandwidth() -> float

  - Client.Send(Message) -> bool

  - Client.Subscribe(func(Message))


- Message

  - Message.GetJson() -> Json

  - Message.GetJsonString() -> string

  - Message.GetBoundTo() -> BoundToKind

  - Message.GetKind() -> MessageKind


- EntitySource

  - EntitySource.GetEntity(uniqueId: int) -> Entity

  - EntitySource.GetAllEntities() -> Entity[]


- Entity

  - Entity.GetOrientation() -> Orientation

  - Entity.GetPosition() -> Position

  - Entity.GetTypeId() -> int

  - Entity.GetUniqueId() -> int


- Logger

  - Logger.Debug(string)

  - Logger.Info(string)

  - Logger.Warn(string)

  - Logger.Error(string)