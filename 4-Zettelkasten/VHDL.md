```VHDL
ENTITY comprehensive_vhdl_design IS
    GENERIC (
        DATA_WIDTH      : natural := 8;
        ACCUM_WIDTH     : natural := 16
    );
    PORT (
        -- Control Ports
        clk             : IN  std_logic;
        reset           : IN  std_logic;
        start_processing: IN  std_logic;
        control_mode    : IN  std_logic_vector(1 DOWNTO 0); -- Example: "00", "01", "10", "11" for different ops

        -- Data Ports
        data_in         : IN  std_logic_vector(DATA_WIDTH-1 DOWNTO 0);
        processed_data_out: OUT std_logic_vector(DATA_WIDTH-1 DOWNTO 0);
        accumulator_out : OUT signed(ACCUM_WIDTH-1 DOWNTO 0); -- Signal processing output
        calculated_parity_out: OUT std_logic;                 -- Output from function

        -- Status Ports
        processing_done : OUT std_logic;
        fsm_current_state_debug: OUT std_logic_vector(2 DOWNTO 0) -- For debugging FSM state
    );
END ENTITY comprehensive_vhdl_design;

ARCHITECTURE behavioral OF comprehensive_vhdl_design IS

    -- FSM State Definition
    TYPE fsm_state_type IS (
        S_IDLE,
        S_LOAD_INPUT,
        S_PROCESS_VECTOR,
        S_ACCUMULATE,
        S_OUTPUT_RESULTS
    );
    SIGNAL current_fsm_state, next_fsm_state : fsm_state_type;

    -- Internal Signals
    SIGNAL internal_data_buffer   : std_logic_vector(DATA_WIDTH-1 DOWNTO 0);
    SIGNAL temp_processed_vector  : std_logic_vector(DATA_WIDTH-1 DOWNTO 0);
    SIGNAL internal_accumulator   : signed(ACCUM_WIDTH-1 DOWNTO 0);
    SIGNAL internal_parity        : std_logic;

    -- Control signals from FSM
    SIGNAL load_en              : std_logic;
    SIGNAL process_en           : std_logic;
    SIGNAL accumulate_en        : std_logic;
    SIGNAL output_en            : std_logic;
    SIGNAL clear_accumulator_en : std_logic;


    -- Function to calculate odd parity (1 if odd number of '1's, 0 if even)
    -- This function is purely combinational and synthesizable.
    FUNCTION calculate_odd_parity (vec : std_logic_vector) RETURN std_logic IS
        VARIABLE parity_val : std_logic := '0'; -- Initialize for even parity
    BEGIN
        -- Loop over the signal vector
        FOR i IN vec'range LOOP
            IF vec(i) = '1' THEN
                parity_val := NOT parity_val; -- Toggle for each '1'
            END IF;
        END LOOP;
        RETURN parity_val;
    END FUNCTION calculate_odd_parity;

    -- Procedure to load data into an internal buffer
    -- This procedure will be called within a clocked process.
    PROCEDURE load_vector_proc (
        SIGNAL target_buffer : OUT std_logic_vector;
        CONSTANT source_data : IN  std_logic_vector
    ) IS
    BEGIN
        target_buffer <= source_data;
    END PROCEDURE load_vector_proc;

BEGIN

    -- FSM State Register (Sequential Logic)
    fsm_state_register_proc : PROCESS (clk, reset)
    BEGIN
        IF reset = '1' THEN
            current_fsm_state <= S_IDLE;
        ELSIF rising_edge(clk) THEN
            current_fsm_state <= next_fsm_state;
        END IF;
    END PROCESS fsm_state_register_proc;

    -- FSM Next State Logic & Control Signal Generation (Combinational Logic)
    fsm_next_state_logic_proc : PROCESS (current_fsm_state, start_processing, internal_data_buffer) -- Add other inputs if needed for transitions
    BEGIN
        -- Default assignments for control signals and next state
        next_fsm_state <= current_fsm_state;
        load_en <= '0';
        process_en <= '0';
        accumulate_en <= '0';
        output_en <= '0';
        processing_done <= '0';
        clear_accumulator_en <= '0';

        CASE current_fsm_state IS
            WHEN S_IDLE =>
                clear_accumulator_en <= '1'; -- Clear accumulator when idle
                IF start_processing = '1' THEN
                    next_fsm_state <= S_LOAD_INPUT;
                END IF;

            WHEN S_LOAD_INPUT =>
                load_en <= '1';
                next_fsm_state <= S_PROCESS_VECTOR;

            WHEN S_PROCESS_VECTOR =>
                process_en <= '1';
                next_fsm_state <= S_ACCUMULATE;

            WHEN S_ACCUMULATE =>
                accumulate_en <= '1';
                next_fsm_state <= S_OUTPUT_RESULTS;

            WHEN S_OUTPUT_RESULTS =>
                output_en <= '1';
                processing_done <= '1';
                IF start_processing = '0' THEN -- Wait for start to go low to allow re-trigger
                    next_fsm_state <= S_IDLE;
                END IF;

            WHEN OTHERS =>
                next_fsm_state <= S_IDLE;
        END CASE;
    END PROCESS fsm_next_state_logic_proc;

    -- Data Path and Processing Logic (Mainly Sequential with some Combinational parts)
    data_processing_proc : PROCESS (clk, reset)
        -- Variable for temporary vector manipulation inside the loop
        VARIABLE temp_vec_var : std_logic_vector(DATA_WIDTH-1 DOWNTO 0);
    BEGIN
        IF reset = '1' THEN
            internal_data_buffer <= (OTHERS => '0');
            temp_processed_vector <= (OTHERS => '0');
            internal_accumulator <= (OTHERS => '0');
            internal_parity <= '0';
        ELSIF rising_edge(clk) THEN
            -- Action based on FSM control signals

            IF clear_accumulator_en = '1' THEN
                internal_accumulator <= (OTHERS => '0');
            END IF;

            IF load_en = '1' THEN
                -- Call procedure to load data
                load_vector_proc(internal_data_buffer, data_in);
            END IF;

            IF process_en = '1' THEN
                -- Calculate parity using the function
                internal_parity <= calculate_odd_parity(internal_data_buffer);

                -- Loop over the signal vector and modify it conditionally
                temp_vec_var := internal_data_buffer; -- Work on a copy

                FOR i IN internal_data_buffer'range LOOP
                    -- Conditional modification based on control_mode and bit value
                    CASE control_mode IS
                        WHEN "00" => -- No change
                            NULL;
                        WHEN "01" => -- Invert bit if it's '1'
                            IF internal_data_buffer(i) = '1' THEN
                                temp_vec_var(i) := NOT internal_data_buffer(i);
                            END IF;
                        WHEN "10" => -- If current bit is '1' and not the last bit, invert next bit
                            IF internal_data_buffer(i) = '1' AND i < DATA_WIDTH-1 THEN
                                temp_vec_var(i+1) := NOT internal_data_buffer(i+1);
                            END IF;
                        WHEN "11" => -- Shift left by 1 (simple example, MSB lost, LSB is '0')
                            IF i < DATA_WIDTH-1 THEN
                                temp_vec_var(i) := internal_data_buffer(i+1);
                            ELSE
                                temp_vec_var(i) := '0'; -- LSB
                            END IF;
                        WHEN OTHERS =>
                            NULL;
                    END CASE;
                END LOOP;
                temp_processed_vector <= temp_vec_var; -- Assign modified vector to signal
            END IF;

            IF accumulate_en = '1' THEN
                -- Signal Processing: Accumulate the processed vector (treat as unsigned for simplicity)
                -- Resize to match accumulator width before addition
                internal_accumulator <= internal_accumulator + signed(resize(unsigned(temp_processed_vector), ACCUM_WIDTH));
            END IF;

            IF output_en = '1' THEN
                -- Assign final values to output ports
                -- This is done in a clocked section to ensure outputs are registered
                -- and stable when processing_done goes high.
                NULL; -- Values are assigned concurrently below from registered signals
            END IF;

        END IF;
    END PROCESS data_processing_proc;

    -- Concurrent assignments for outputs (driven by registered internal signals)
    processed_data_out <= temp_processed_vector WHEN output_en = '1' ELSE (OTHERS => 'Z'); -- Output valid when output_en is high
    accumulator_out    <= internal_accumulator WHEN output_en = '1' ELSE (OTHERS => 'Z');
    calculated_parity_out <= internal_parity WHEN output_en = '1' ELSE 'Z';


    -- FSM state debug output (combinational mapping from enum to vector)
    fsm_state_debug_output_proc: PROCESS(current_fsm_state)
    BEGIN
        CASE current_fsm_state IS
            WHEN S_IDLE           => fsm_current_state_debug <= "000";
            WHEN S_LOAD_INPUT     => fsm_current_state_debug <= "001";
            WHEN S_PROCESS_VECTOR => fsm_current_state_debug <= "010";
            WHEN S_ACCUMULATE     => fsm_current_state_debug <= "011";
            WHEN S_OUTPUT_RESULTS => fsm_current_state_debug <= "100";
            WHEN OTHERS           => fsm_current_state_debug <= "111"; -- Error/undefined state
        END CASE;
    END PROCESS fsm_state_debug_output_proc;

END ARCHITECTURE behavioral;

```