<script>
    // Estado inicial usando las nuevas Runes de Svelte 5
    let tipoServicio = $state("oficina");
    let frecuencia = $state("semanal");
    let metrosCuadrados = $state(50);
    let ventanas = $state(false);
    let alfombras = $state(false);

    // Configuraciones fijas
    const preciosBase = { oficina: 100, local: 150, industrial: 300 };
    const multiplicadores = { semanal: 1, quincenal: 1.2, mensual: 1.5 };

    // Estado derivado (Equivalente al computed de Vue o función de Alpine)
    // Svelte 5 analiza esto y lo re-calcula mágicamente con una Rune
    let calcularTotal = $derived.by(() => {
        let base = preciosBase[tipoServicio]; // Aquí NO usas .value ni "this", usas la variable directa
        let porMetro = metrosCuadrados * 0.5;
        let extras = (ventanas ? 30 : 0) + (alfombras ? 50 : 0);

        return (
            (base + porMetro + extras) *
            multiplicadores[frecuencia]
        ).toFixed(2);
    });
</script>

<div class="p-6 bg-white rounded-xl shadow-md max-w-lg mx-auto">
    <h3 class="text-xl font-bold mb-4">Calculadora Svelte 5</h3>

    <div class="mb-4">
        <select bind:value={tipoServicio} class="w-full p-2 border rounded-md">
            <option value="oficina">Oficina Estándar</option>
            <option value="local">Local Comercial</option>
        </select>
    </div>

    <div class="mb-4">
        <label class="block text-sm mb-1">Tamaño: {metrosCuadrados} m²</label>
        <input
            type="range"
            min="20"
            max="500"
            step="10"
            bind:value={metrosCuadrados}
            class="w-full"
        />
    </div>

    <div class="mb-6">
        <label class="flex items-center">
            <input type="checkbox" bind:checked={ventanas} class="rounded" />
            <span class="ml-2 text-sm"
                >Limpieza profunda de ventanas (+$30)</span
            >
        </label>
    </div>

    <div class="bg-blue-50 p-4 rounded-lg text-center">
        <span class="text-3xl font-black text-blue-600">${calcularTotal}</span>
    </div>
</div>
