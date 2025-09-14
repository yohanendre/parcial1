Yohan Andres Montes Herrera 
conclusion:
1). En los primeros tres numeros de rec en time  que van de 4 a 15 se ve en como va desde cero en 4 siendo haciendo muy rapido esta operacion
no estante se ve como comienza un crecimiento exponencial en el tiempo de ejecucion que vas desde el 37 a 42 estos comienzan a consumir 
mas tiempo en la repeticion de calculos en comparacion a los primeros numeros.

2). En peak se puede ver lo mismo que el tiempo un pricipio con poco consumo de memoria pero cuando n comienza a aumentar el consumo se hace mayor 
teniendo picos de consumo muy grandes  

3). En la grafica podemos ver a time y a B actuando vemos como time se mentiene casi lineal en mayoria del tiempo
pero del 37 al 42 teniendo una subida visaible.
En peak podemos ver algo muy diferente ya que todo esta igual con time pero podemos ver como comienza una pequeña separacion 
poco notable pero ya de 15 a 22 hay una notable  separacion comenzando una subida hasta que vuelve a bajar para el 42.

Esto nos muestra como fibonacci nos puede servir para caculos pequeños los cuales hare supere rapido y sin consumir almacenamiento
pero ya para caculos grandes la demora es mayor asi como el consumo de mayor almacenamiento 
pudiendo llevar nuestros equipos a no responder por la gran presion y estres que se le somete.


def measure_mem(func:Callable[[int], int], n:int, repeat:int=3) -> Tuple[int, float]:
    # Retorna (pico_bytes, tiempo_promedio)
    times = []
    peak = 0
    for _ in range(repeat):
        tracemalloc.start()
        t0 = time.perf_counter()
        res = func(n)
        dt = time.perf_counter() - t0
        current, peak_bytes = tracemalloc.get_traced_memory()
        tracemalloc.stop()
        times.append(dt)
        peak = max(peak, peak_bytes)
        # usar res para evitar optimizaciones triviales
        if res < 0:
            print("imposible")
    return peak, statistics.mean(times)
vector_time = []
vector_rec = []


   
for n in (4, 7, 15, 22, 27, 34, 37, 42):
    print(f"n={n}")
    # cuidado: fib_recursive crece muy rápido
    peak_r, t_r = measure_mem(fib_recursive, n, repeat=1)
    vector_rec.append(peak_r)
    vector_time.append(t_r)
    peak_m, t_m = measure_mem(fib_memo, n, repeat=3)
    peak_i, t_i = measure_mem(fib_iter, n, repeat=3)
    print(f"  rec  peak={peak_r}B  time≈{t_r:.4f}s")
    print(f"  memo peak={peak_m}B  time≈{t_m:.6f}s")
    print(f"  iter peak={peak_i}B  time≈{t_i:.6f}s")




    import plotly.graph_objs as go
# Datos de ejemplo obtenidos de la celda anterior (ajusta si tienes otros valores):
sizes2 = [4, 7, 15, 22, 27, 34, 37, 42]#sizes = [1_000, 5_000, 10_000, 20_000]

for n in sizes2:
    data = sorted(rand_list(n, unique=True))
    print(f"\nLista ordenada de {n} elementos")
    target = data[-1]  # peor caso aprox. para lineal
    mean_lin, s_lin = timeit(linear_search, data, target, repeat=8)
    mean_bin, s_bin = timeit(binary_search, data, target, repeat=8)
    vector_rec.append(peak_r)
    vector_time.append(t_r)
fig = go.Figure()
fig.add_trace(go.Scatter(x=sizes2, y=vector_time, mode='lines+markers', name='time=s (O(n))'))
fig.add_trace(go.Scatter(x=sizes2, y=vector_rec, mode='lines+markers', name='peak=B (O(log n))'))

fig.update_layout(title='Comparación de tiempos: yohan andres montes herrera ',
                  xaxis_title='tamaño de la lista (n)',
                  yaxis_title='Tiempo promedio (s)',
                  legend=dict(x=0.01, y=0.99))