# Pencarian data dan Pagination

Nama: Fauzan Alfariz

NIM: 312410620

Kelas: TI.24.A4

## Index.php
```php
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

include 'koneksi.php';

$limit = 2;
$halaman = isset($_GET['hal']) ? (int)$_GET['hal'] : 1;
if ($halaman < 1) $halaman = 1;

$awalData = ($halaman - 1) * $limit;

$cari = isset($_GET['cari']) ? mysqli_real_escape_string($koneksi, $_GET['cari']) : '';
$where = ($cari != '') ? "WHERE nama LIKE '%$cari%' OR kategori LIKE '%$cari%'" : "";


$sql = "SELECT * FROM data_barang $where ORDER BY id_barang ASC LIMIT $awalData, $limit";
$result = mysqli_query($koneksi, $sql);


$totalQuery = mysqli_query($koneksi, "SELECT COUNT(*) AS total FROM data_barang $where");
$totalRow = mysqli_fetch_assoc($totalQuery);
$totalData = $totalRow['total'];
$totalHalaman = ceil($totalData / $limit);
?>

<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Data Barang</title>
<link rel="stylesheet" href="style.css">

<style>
/* ================= SEARCH ================= */
.search-box {
    display: flex;
    gap: 10px;
    width: 50%;
    margin: 20px 0;
}

.search-box input {
    flex: 1;
    padding: 10px 14px;
    border-radius: 8px;
    border: 1px solid #005bbb;
    outline: none;
    font-size: 14px;
}

.search-box input:focus {
    border-color: #003f7f;
    box-shadow: 0 0 0 2px rgba(0, 91, 187, 0.2);
}

.search-box button {
    padding: 10px 24px;
    background: #005bbb;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
}

.search-box button:hover {
    background: #004a9f;
}


/* ================= PAGINATION ================= */
.bottom-area {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 35px;
    padding-bottom: 20px;
}

.pagination {
    display: flex;
    gap: 8px;
    margin-bottom: 12px;
}

.pagination a {
    padding: 8px 14px;
    background: #005bbb;
    color: #fff;
    border-radius: 8px;
    text-decoration: none;
    font-weight: bold;
    transition: 0.3s;
}

.pagination a:hover {
    background: #004a9f;
}

.pagination a.active {
    background: #003f7f;
}


/* ================= FOOTER ================= */
footer {
    font-size: 13px;
    color: #003f7f;
    text-align: center;
}
</style>
</head>

<body>

<div class="container">

    <h1>Data Barang</h1>

    <a href="tambah.php" class="btn">+ Tambah Barang</a>

    <!-- SEARCH -->
    <form method="GET" class="search-box">
        <input type="text" name="cari" placeholder="Cari nama barang"
               value="<?= htmlspecialchars($cari); ?>">
        <button type="submit">Cari</button>
    </form>

    <!-- TABEL -->
    <table>
        <tr>
            <th>No</th>
            <th>Gambar</th>
            <th>Nama</th>
            <th>Kategori</th>
            <th>Harga Beli</th>
            <th>Harga Jual</th>
            <th>Stok</th>
            <th>Aksi</th>
        </tr>

        <?php
        $no = $awalData + 1;
        while ($row = mysqli_fetch_assoc($result)) :
        ?>
        <tr>
            <td><?= $no++; ?></td>
            <td>
                <?php if (!empty($row['gambar'])) : ?>
                    <img src="gambar/<?= $row['gambar']; ?>" class="thumb">
                <?php else : ?>
                    -
                <?php endif; ?>
            </td>
            <td><?= $row['nama']; ?></td>
            <td><?= $row['kategori']; ?></td>
            <td><?= number_format($row['harga_beli']); ?></td>
            <td><?= number_format($row['harga_jual']); ?></td>
            <td><?= $row['stok']; ?></td>
            <td>
                <a class="link" href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a> |
                <a class="link"
                   href="hapus.php?id=<?= $row['id_barang']; ?>"
                   onclick="return confirm('Yakin hapus data?')">
                   Hapus
                </a>
            </td>
        </tr>
        <?php endwhile; ?>
    </table>

    <!-- PAGINATION + FOOTER -->
    <div class="bottom-area">

        <div class="pagination">
            <?php if ($halaman > 1) : ?>
                <a href="?hal=<?= $halaman - 1; ?>&cari=<?= $cari; ?>">Prev</a>
            <?php endif; ?>

            <?php for ($i = 1; $i <= $totalHalaman; $i++) : ?>
                <a href="?hal=<?= $i; ?>&cari=<?= $cari; ?>"
                   class="<?= ($halaman == $i) ? 'active' : ''; ?>">
                   <?= $i; ?>
                </a>
            <?php endfor; ?>

            <?php if ($halaman < $totalHalaman) : ?>
                <a href="?hal=<?= $halaman + 1; ?>&cari=<?= $cari; ?>">Next</a>
            <?php endif; ?>
        </div>

        <footer>
            © 2025 · Pemograman Web 1
        </footer>

    </div>

</div>

</body>
</html>
```

## Koneksi.php
```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db   = "latihan1"; 
$koneksi = mysqli_connect($host, $user, $pass, $db);

if (!$koneksi) {
    die("Koneksi gagal: " . mysqli_connect_error());
}
?>
```



## Hasil 
<img width="1919" height="1079" alt="Screenshot 2025-12-30 194753" src="https://github.com/user-attachments/assets/777b0cd4-71b9-4b92-b0e6-c20266d06ec6" />

<img width="1917" height="659" alt="image" src="https://github.com/user-attachments/assets/f7525e91-2aca-427d-b0d8-fa0937d69637" />


