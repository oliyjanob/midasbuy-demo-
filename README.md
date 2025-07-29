function loadOrders() {
        const list = document.getElementById("ordersList");
        list.innerHTML = "";
        orders.filter(o => o.username === currentUser.username).forEach(o => {
            const li = document.createElement("li");
            li.textContent = ${o.amount} - ${o.status};
            list.appendChild(li);
        });
    }

    function logout() {
        currentUser = null;
        document.getElementById("mainPanel").style.display = "none";
        document.getElementById("userCard").style.display = "block";
    }

    // Admin funksiyalari
    function adminLogin() {
        const u = document.getElementById("adminUser").value.trim();
        const p = document.getElementById("adminPass").value.trim();
        const admin = users.find(x => x.username === u && x.password === p && x.role === "admin");
        if (!admin) return alert("Admin ma’lumotlari noto‘g‘ri!");

        adminLogged = true;
        document.getElementById("adminLoginCard").style.display = "none";
        document.getElementById("adminPanel").style.display = "block";
        loadUsers();
    }

    function loadUsers() {
        const tbody = document.querySelector("#usersTable tbody");
        tbody.innerHTML = "";
        users.filter(u => u.role === "user").forEach(u => {
            const tr = document.createElement("tr");
            tr.innerHTML = 
                <td>${u.username}</td>
                <td>${u.balance}</td>
                <td>
                    <button onclick="resetPassword('${u.username}')">Parolni tiklash</button>
                    <button onclick="addBalance('${u.username}')">Balans qo‘shish</button>
                </td>;
            tbody.appendChild(tr);
        });
    }

    function resetPassword(username) {
        const newPass = prompt("Yangi parolni kiriting:");
        if (!newPass) return;
        const user = users.find(u => u.username === username);
        if (user) {
            user.password = newPass;
            alert("Parol yangilandi!");
        }
    }

    function addBalance(username) {
        const add = Number(prompt("Qancha qo‘shasiz?"));
        if (!add || add <= 0) return;
        const user = users.find(u => u.username === username);
        if (user) {
            user.balance += add;
            alert("Balans yangilandi!");
            loadUsers();
        }
    }

    function logoutAdmin() {
        adminLogged = false;
        document.getElementById("adminPanel").style.display = "none";
        document.getElementById("userCard").style.display = "block";
    }
</script>
</body>
</html>
