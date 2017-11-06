# node-red-contrib-frp
A collection of Node-RED nodes that use FRP to perform stream manipulation.

## frp-map  
This node maps <code>msg.payload</code> using the specified mapping function.
### Inputs
- <code>payload</code>: Data inside the incoming message. Can be renamed to any variable name you prefer in the mapping function
### Outputs
- <code>payload</code>: Value returned by the node's mapping function.
- <code>topic</code>: A mandatory property used to identify the origin of a message. This is used by the combine nodes to process multiple inputs, and should not contain spaces or periods.
### Mapping Function
The <code>frp-map</code> node allows you to define the body of the internal <code>mappingFunction</code>. Naming the function is not necessary as it is already defined internally. The mapping function can only contain <b>one</b> input parameter and is <b>required</b> to return a value, which is stored in the message's <code>payload</code>.

## frp-filter  
This node allows incoming messages to pass through if it fulfills the criteria specified in the filtering function.

### Inputs
- <code>payload</code>: Data inside <code>msg.payload</code> of the incoming message. Can be renamed to any variable name you prefer in the filtering function.
### Outputs
- <code>topic</code>: A mandatory property used to identify the origin of a message. This is used by the combine nodes to process multiple inputs, and should not contain spaces or periods.
### Filtering Function
The <code>frp-filter</code> node allows you to define the body of the internal <code>filteringFunction</code>. Naming the function is not necessary as it is already defined internally.
The filtering function discerns if the data inside the message (contained in <code>msg.payload</code>) passes a certain criteria. Therefore, the function should have one input parameter and return either <code>true</code> or <code>false</code>. It's also important to note that <code>frp-filter</code> does not change the contents of the message, and only decides whether or not to pass it along.


## frp-scan  
This node performs a fold operation for every incoming message: it takes the value of <code>msg.payload</code> and runs it through the accumulator function, storing and returning an accumulated value from the first to the latest input.

### Inputs
- <code>payload</code>: The initial value used for accumulation.
- <code>payload</code>: Data inside <code>msg.payload</code> of the incoming message. Can be renamed to any variable name you prefer in the accumulator function.
- <code>accumulatedValue</code>: Value obtained from computing the previous message with the accumulator function. Can be renamed to any variable name you prefer in the accumulator function.

### Outputs
- <code>payload</code>: Contains the accumulated value obtained from the accumulator function.
- <code>topic</code>: A mandatory property used to identify the origin of a message. This is used by the combine nodes to process multiple inputs, and should not contain spaces or periods.

### Accumulator Function
<code>frp-scan</code> requires you to define the body of the internal <code>accumulatorFunction</code>. Naming the function is not necessary as it is already defined internally. The accumulator function must have two parameters: the first being <code>accumultedValue</code>, followed by <code>payload</code>. For the very first run, the <code>seed value</code> serves as the accumulated value, and the result is then stored and used for subsequent runs. This function must therefore return a value in order for it to work properly.

## frp-combineAsArray
This node combines the <code>msg.payload</code>'s of its inputs into an array and outputs it.

### Inputs
- <code>input topics</code>: A list containing the topics of the input nodes that can be reordered.

### Outputs
- <code>payload</code>: Contains the accumulated value obtained from the accumulator function.
- <code>topic</code>: A mandatory property used to identify the origin of a message. This is used by the combine nodes to process multiple inputs, and should not contain spaces or periods.

## frp-combineWith 
This node combines the <code>msg.payload</code>'s of its inputs into an array and outputs it.

### Inputs
- <code>input topics</code>: A list containing the topics of the input nodes that can be reordered.
- <code>payload1, payload2, ...</code>: Data contained in the <code>msg.payload</code>'s of the input nodes. Their order should be the same as that of <code>input topics</code> 

### Outputs
- <code>payload</code>: Contains the accumulated value obtained from the accumulator function.
- <code>topic</code>: A mandatory property used to identify the origin of a message. This is used by the combine nodes to process multiple inputs, and should not contain spaces or periods.

### Aggregator Function
<code>frp-combineWith</code> requires you to define the body of the internal <code>aggregateFunction</code>. Naming the function is not necessary as it is already defined internally. The aggregator function must have as many parameters as there are input topics, and must observe the correct order. It must also return a value for it to work properly.
